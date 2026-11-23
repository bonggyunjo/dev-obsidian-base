---
description: Roomora PostgreSQL EXPLAIN ANALYZE 및 k6 부하테스트 실측 성능 기록.
layer: L3
related:
  - ../01_Core_Rules/system_architecture.md
  - ../01_Core_Rules/test_strategy.md
---

# Roomora Performance Benchmarks

## 1. PostgreSQL `EXPLAIN (ANALYZE, BUFFERS)` (2026-11-24 실측)
> 테스트 환경: Docker `pgvector/pgvector:pg16` (`roomora-postgres`), 트랜잭션 내 대용량 시드 삽입 후 `ANALYZE` 측정 및 `ROLLBACK`.

| 시나리오 | 데이터 규모 | 실행 계획 | Execution Time | Shared Buffers |
| :--- | :--- | :--- | ---: | ---: |
| **Keyset 페이징** (`id < 25000 ORDER BY id DESC LIMIT 20`) | 50,000건 | `Index Scan Backward using recipe_post_pkey` | **0.060 ms** | `hit=6` |
| **Offset 페이징** (`OFFSET 40000 LIMIT 20`) | 50,000건 | `Index Scan Backward` (40,020건 폐기) | **12.021 ms** | `hit=948` |
| **복합 필터 + Keyset** (`ONE_ROOM`, `no_drill`, `budget <= 500000`) | 50,000건 | `Index Scan Backward using recipe_post_pkey` | **0.029 ms** | `hit=3` |
| **콘테스트 DB 정렬** (`round_id`, `vote_count DESC LIMIT 10`) | 2,000건 | `Index Scan using idx_contest_post_ranking` + `Incremental Sort` | **0.055 ms** | `hit=17` |
| **`pgvector` HNSW 유사도 검색** (`1536d`, `LIMIT 10`) | 2,000건 | `Index Scan using idx_rag_chunk_embedding_hnsw` | **2.920 ms** | `hit=160` |

- **핵심 결론**: 50,000건 기준 Keyset 페이징은 깊은 페이지(`OFFSET 40000`) 대비 **약 200배 빠른 실행 시간(0.060ms vs 12.021ms)** 과 **158배 적은 버퍼 접근(`6` vs `948`)** 을 기록했다.

---

## 2. `k6` 부하테스트 실측 (2026-11-24)
> 테스트 환경: Docker `grafana/k6:latest` → `roomora-bench` (Spring Boot 3.3.5, `AI_API_KEY` 미설정 로컬 임베딩/캐시 모드, 각 시나리오 **20 VUs × 10초**).

| 시나리오 | 엔드포인트 | 완료 요청 수 | 처리량 (RPS) | Avg | Med | P90 | P95 | 에러율 |
| :--- | :--- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| **레시피 Keyset 목록** | `GET /api/recipes?sort=LATEST&size=20` | ~15,669 | ~1,567/s | 12.48ms | 10.44ms | 19.33ms | **24.16ms** | **0.00%** |
| **콘테스트 Redis ZSET 랭킹** | `GET /api/contests/rounds/1/ranking?limit=10` | ~18,063 | ~1,806/s | 10.85ms | 9.98ms | 15.89ms | **18.21ms** | **0.00%** |
| **RAG 검색 (Redis Cache Hit)** | `POST /api/search/rag` | ~34,965 | ~3,496/s | 5.51ms | 5.04ms | 8.21ms | **9.53ms** | **0.00%** |

- **전체 합계**: 총 **68,697건** 요청, 평균 **2,146.39 req/s**, 실패 **0건(0.00%)**.
- **RAG Cold vs Cache Hit 비교**:
  - Live Gemini (`gemini-flash-latest`) Cold 호출 (1단계 실측): **21,265ms**
  - 오프라인 로컬 임베딩 Cold 호출 (5단계 실측): **1,905ms**
  - Redis Cache Hit (`k6` 20 VUs 부하 상태): **P95 9.53ms** (Live Gemini 대비 **약 2,231배** 단축)
