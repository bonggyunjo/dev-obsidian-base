---
description: 마이페이지 내 활동 모아보기(레시피·출품작·좋아요·북마크·댓글), 프로필 관리 및 팔로우/팔로잉 구현 시 열람 필수.
layer: L2
related:
  - ../01_Core_Rules/erd_schema.md
  - ./auth_oauth_spec.md
  - ./recipe_community_spec.md
  - ./contest_ranking_spec.md
---

# MyPage & Social(Follow) 기능 명세서

## 1. 요구사항 (REQ)
- **REQ-MYP-001**: 사용자는 본인 프로필(닉네임, 프로필 이미지, 한줄 소개)을 조회·수정할 수 있으며 회원 탈퇴(Soft Delete)를 요청할 수 있다.
- **REQ-MYP-002**: 다른 유저를 팔로우/언팔로우(`user_follow`)할 수 있으며 자기 자신 팔로우는 차단한다.
- **REQ-MYP-003**: 특정 유저의 팔로워 목록·팔로잉 목록 및 타 유저 공개 프로필(닉네임, 프로필 이미지, 소개, 팔로워/팔로잉 수, 팔로우 여부)을 조회할 수 있다. (타 유저별 작성 레시피/출품작 공개 목록 필터는 향후 확장)
- **REQ-MYP-004**: 마이페이지에서 내가 작성한 레시피·출품작·댓글, **내가 좋아요한 레시피**, **내가 북마크한 레시피/콘테스트 출품작**을 분리 조회할 수 있다.

## 2. API / 데이터 계약
- `GET /api/users/me` / `PATCH /api/users/me` (`{ nickname?, profileImageUrl?, bio? }`) / `DELETE /api/users/me`
- `GET /api/users/{userId}` — 타 유저 공개 프로필 (닉네임, 프로필 이미지, 소개, 팔로워/팔로잉 수, 팔로우 여부)
- `POST /api/users/{userId}/follow` — 팔로우/언팔로우 토글 (`follower_count`, `following_count` 동기화)
- `GET /api/users/{userId}/followers` / `GET /api/users/{userId}/followings` (페이징)
- `GET /api/mypage/recipes` / `GET /api/mypage/contests` / `GET /api/mypage/comments`
- `GET /api/mypage/likes/recipes`
- `GET /api/mypage/scraps?type=RECIPE|CONTEST`

## 3. 예외 처리 및 엣지 케이스
- 자기 자신(`targetUserId == currentUser.id`) 팔로우 요청 시 `400 Bad Request` 반환.
- 탈퇴한 유저(`deleted_at IS NOT NULL`) 조회 또는 팔로우 요청 시 `404 Not Found` 반환.
- 닉네임 변경 시 다른 활성 유저가 이미 사용 중이면 `409 Conflict` 반환.

## 4. 검증 기준 (DoD)
- 자기 자신 팔로우 차단(`400`) 및 중복 팔로우 동시성 테스트 통과
- 좋아요/북마크(레시피·콘테스트 구분) 마이페이지 조회 인덱스·페이징 테스트 통과
