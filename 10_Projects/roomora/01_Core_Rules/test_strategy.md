---
description: Roomora 도메인별 검증·테스트 전략, QA 에이전트 3단 검증 프로세스 및 k6 부하 테스트 기준서.
layer: L1
related:
  - ./system_architecture.md
  - ./erd_schema.md
  - ../../../30_Agent_Playbooks/qa_test_agent.md
---

# Verification & Test Strategy — Roomora

## 1. 기본 원칙
- **실제 DB/Redis 기반 통합 테스트**: H2 인메모리 DB는 `pgvector(HNSW)`, `pg_trgm`, Partial Unique Index(`WHERE deleted_at IS NULL`)를 지원하지 않으므로, 통합 테스트는 실제 `pgvector/pgvector:pg16` + `redis:7` 컨테이너(로컬 Docker / CI Service)를 사용한다.
- **AI 비용 0원 테스트**: 단위·통합 테스트에서는 `EmbeddingModel` / `ChatClient`를 Mock/Stub으로 대체해 외부 유료 API 호출 없이 파이프라인·Fallback을 검증한다.

---

## 2. 최종 반영 전 5단계 검증 프로세스
1. **구현 & 기본 검증 (`Backend / Frontend Agent`)**: `plan.md` 기준 프로덕션 코드 + 정상 흐름 테스트 작성 및 실행
2. **독립 엣지케이스 검증 (`QA / Test Agent`)**: 구현자와 분리된 컨텍스트에서 `src/test/`만 소유해 실패·경계값·동시성 테스트 추가 및 실행 (실패 시 테스트 완화 금지, 구현 에이전트가 `src/main/` 수정)
3. **최종 감사 (`Gate 2: Read-Only Reviewer`)**: `git diff`와 `REQ-nnn` 대조 후 PASS / REJECT 판정
4. **로컬 & CI 관문**: `scripts/verify.ps1` + GitHub Actions CI(`Gitleaks` + `pgvector`/`Redis` 테스트 + `nuxi typecheck`/`nuxt build`) 통과
5. **세션 마무리 (`Session Handoff`)**: 버그 해결 내역 `troubleshooting.md` 기록 + `activeContext.md` 50줄 덮어쓰기

---

## 3. 도메인별 필수 검증 기준

| 도메인 | 검증 도구/방식 | 필수 테스트 시나리오 |
| :--- | :--- | :--- |
| **Auth / Security** | `@WebMvcTest` + 통합 테스트 | 비인증(401)·타인 수정(403) 차단, Refresh Token Rotation 재사용 탐지, OAuth2 Mock 가입 및 탈퇴 후 재가입(Partial Unique) |
| **Recipe 커뮤니티** | JPA/QueryDSL 통합 테스트 | 복합 조건(주거형태·무타공·예산) + Keyset(No-Offset) 페이징 정합성, `EXPLAIN ANALYZE` 인덱스 사용 확인 |
| **Contest 동시성 & 랭킹** | `ExecutorService` + `CountDownLatch` | **동시 100스레드 투표** 시 `UNIQUE (round_id, user_id)` 기반 1표만 성공 및 `vote_count` 100% 정합성, Redis ZSET ↔ 마감 스냅샷 일치 |
| **AI RAG 통합 검색** | Mock LLM + 실제 `pgvector` | Pre-filtering(`예산/무타공`) + 벡터 정렬 정확성, 비동기 재임베딩 멱등성(`chunk_index`), **Resilience4j Fallback(200 OK 우회)** |
| **Frontend (`roomora-web`)** | CI Typecheck & SSR Build | `npx nuxi typecheck` + `npm run build` (Playwright E2E는 전체 기능 연결 후 마지막에 스모크 추가) |

---

## 4. 성능 · 부하 테스트 (`k6` — 포트폴리오 수치화)
1. **레시피 목록 조회**: 일반 Offset 깊은 페이지 vs Keyset + 복합 인덱스 (TPS · P95 비교)
2. **콘테스트 랭킹 & 투표**: DB `ORDER BY` vs Redis ZSET 조회 응답 시간 및 동시 투표 스파이크 테스트
3. **RAG 검색 캐싱**: Redis 캐시 Miss vs Hit 응답 시간 비교
