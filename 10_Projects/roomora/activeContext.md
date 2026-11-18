---
description: Roomora 프로젝트 현재 작업 상태 인수인계서 (50줄 이내).
layer: L0.5
related:
  - ./00_MOC/project_moc.md
---

# Active Context — Roomora

## 현재 단계
- **백엔드 구현 진행 중**: 마일스톤 1(DDL/Docker) · 마일스톤 2(Auth & OAuth2) 완료 및 테스트 통과

## 방금 완료한 일
- `docker-compose.yml` (`pgvector/pgvector:pg16` + `redis:7-alpine`) + `V1__init.sql` (18개 테이블) + `FlywayMigrationTest` 통과
- **Auth & OAuth2 구현 및 통합 테스트 통과** (`AuthIntegrationTest` 4건 + `SmokeTest` + `FlywayMigrationTest`)
- `ERR-001` (Windows 5432 포트 충돌) [troubleshooting.md](./04_ADR_and_Logs/troubleshooting.md) 기록

## 다음 할 일 (Next)
1. **마일스톤 3: Recipe & Community 도메인 구현** (`recipe_community_spec.md` 기반 CRUD, 조건 필터 + Keyset 페이징, 좋아요/스크랩, 2-Depth 대댓글, `AFTER_COMMIT` 이벤트)
2. **마일스톤 4: Contest & Ranking 구현** (회차당 1표 동시성 제어, Redis ZSET 랭킹, 마감 배치)
3. **마일스톤 5: AI RAG 통합 검색 & Checklist 구현**

## 미결정 / 주의사항
- `roomora-server` IntelliJ 호스트 실행 시 `5432` 포트 충돌 주의 (`ERR-001`)
