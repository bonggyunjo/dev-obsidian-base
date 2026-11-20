---
description: Roomora 프로젝트 현재 작업 상태 인수인계서 (50줄 이내).
layer: L0.5
related:
  - ./00_MOC/project_moc.md
---

# Active Context — Roomora

## 현재 단계
- **백엔드 구현 진행 중**: 마일스톤 1~5(DDL · Auth · Recipe · Contest · AI RAG & Checklist) 완료

## 방금 완료한 일
- **마일스톤 5 (AI RAG 통합 검색 & Checklist) 완료** — `Gate 1` → 구현 → `QA Test Agent` → `Gate 2` (`PASS`)
  - 메타데이터 Prefix 청킹 + `ON CONFLICT` 멱등 색인, `pgvector` + `pg_trgm` Hybrid Search(Pre-filtering + RRF), 단계적 조건 완화, Redis 캐시/Rate Limit(429), Resilience4j Fallback, `<document>` 프롬프트 격리, 1-Click 체크리스트 CRUD/진행률
  - 전체 테스트 **23/23 통과** (`RagAndChecklistIntegrationTest` 7건 포함)

## 다음 할 일 (Next)
1. **마일스톤 6: MyPage & Follow 소셜 구현** (`mypage_social_spec.md`)
   - 내 프로필·탈퇴, 팔로우/언팔로우(자기 자신 차단), 타 유저 공개 프로필, 내 글/좋아요/북마크(레시피·콘테스트) 모아보기
2. **시드 데이터 적재 + `roomora-web` (Nuxt 3) 프론트엔드 구현 + `k6` 부하 테스트**

## 미결정 / 주의사항
- `roomora-server` IntelliJ 호스트 실행 시 `5432` 포트 충돌 주의 (`ERR-001`)
