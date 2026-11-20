---
description: Roomora 프로젝트 현재 작업 상태 인수인계서 (50줄 이내).
layer: L0.5
related:
  - ./00_MOC/project_moc.md
---

# Active Context — Roomora

## 현재 단계
- **백엔드 구현 진행 중**: 마일스톤 1~4(DDL · Auth · Recipe · Contest) 완료 및 멀티 에이전트 검증 통과

## 방금 완료한 일
- **마일스톤 4 (Contest & Ranking) 완료** — 구현 → `QA Test Agent` → `Gate 2 Reviewer` (`PASS`)
  - 회차 관리, 1인 1출품(다중 이미지), `DEC-004` 회차당 1표 100스레드 동시성 제어, Redis ZSET 실시간 랭킹, 북마크, 2-Depth 대댓글, 마감 배치(`final_rank`)
  - 전체 테스트 **16/16 통과** (`AuthIntegrationTest` 4 + `RecipeIntegrationTest` 4 + `ContestIntegrationTest` 6 + `FlywayMigrationTest` 1 + `SmokeTest` 1)

## 다음 할 일 (Next)
1. **마일스톤 5: AI RAG 통합 검색 & Checklist 구현** (`rag_search_spec.md`, `checklist_spec.md`)
   - 메타데이터 Prefix 청킹 + 비동기 색인, `pgvector` HNSW + `pg_trgm` Hybrid Search, Redis 캐시/Rate Limit, Resilience4j Fallback, 1-Click 체크리스트 저장
2. **마일스톤 6: MyPage & Follow 소셜 구현** (`mypage_social_spec.md`)

## 미결정 / 주의사항
- `roomora-server` IntelliJ 호스트 실행 시 `5432` 포트 충돌 주의 (`ERR-001`)
