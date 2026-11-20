---
description: Roomora 프로젝트 현재 작업 상태 인수인계서 (50줄 이내).
layer: L0.5
related:
  - ./00_MOC/project_moc.md
---

# Active Context — Roomora

## 현재 단계
- **백엔드 전 도메인 완료 → 프론트엔드(`roomora-web`) & 시드/성능 검증 진입**

## 방금 완료한 일
- **마일스톤 6 (MyPage & Follow 소셜) 완료** — 구현 → `QA Test Agent` → `Gate 2` (`PASS`)
  - 프로필 조회·수정·탈퇴(토큰 블랙리스트), 팔로우/언팔로우(10스레드 동시성 정합성), 팔로워/팔로잉 페이징, 내 레시피·출품작·댓글·좋아요·북마크(`RECIPE`/`CONTEST`) 분리 조회
- **백엔드 전체 테스트 28/28 통과** (`Auth` 4 + `Recipe` 4 + `Contest` 6 + `RAG/Checklist` 7 + `MyPage/Follow` 5 + `Flyway` 1 + `Smoke` 1)

## 다음 할 일 (Next)
1. **초기 시드 데이터(`InteriorGuide` + 샘플 `RecipePost` + `ContestRound`) 및 RAG 색인 배치** 구성
2. **`roomora-web` (Nuxt 3 SSR) 프론트엔드 전체 구현** (`Pinia`, `Tailwind`, `/`, `/search`, `/recipes`, `/contest`, `/mypage`, `/login`)
3. **`k6` 부하 테스트 + `EXPLAIN ANALYZE` 수치화** (포트폴리오 증빙)

## 미결정 / 주의사항
- `roomora-server` IntelliJ 호스트 실행 시 `5432` 포트 충돌 주의 (`ERR-001`)
