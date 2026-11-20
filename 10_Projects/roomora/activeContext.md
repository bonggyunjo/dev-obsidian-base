---
description: Roomora 프로젝트 현재 작업 상태 인수인계서 (50줄 이내).
layer: L0.5
related:
  - ./00_MOC/project_moc.md
---

# Active Context — Roomora

## 현재 단계
- **백엔드 구현 진행 중**: 마일스톤 1(DDL) · 마일스톤 2(Auth) · **마일스톤 3(Recipe & Community)** 완료 및 테스트 통과

## 방금 완료한 일
- **Recipe & Community 구현 및 통합 테스트 통과** (`RecipeIntegrationTest` 4건 포함 전체 10건 통과)
  - 조건별 레시피 CRUD, Presigned URL 업로드, 복합 필터 + Keyset 페이징, 좋아요/스크랩, 2-Depth 대댓글, Redis 조회수 중복 방지, `AFTER_COMMIT` RAG 색인 이벤트

## 다음 할 일 (Next)
1. **마일스톤 4: Contest & Ranking 구현** (`contest_ranking_spec.md` 기반 회차 관리, 출품작 CRUD, 회차당 1표 동시성 100스레드 검증, Redis ZSET 실시간 랭킹, 마감 배치)
2. **마일스톤 5: AI RAG 통합 검색 & Checklist 구현**
3. **마일스톤 6: MyPage & Follow 소셜 구현**

## 미결정 / 주의사항
- `roomora-server` IntelliJ 호스트 실행 시 `5432` 포트 충돌 주의 (`ERR-001`)
