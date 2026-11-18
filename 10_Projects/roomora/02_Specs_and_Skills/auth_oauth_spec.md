---
description: 회원가입, 로그인, JWT Refresh Token Rotation, OAuth2(카카오/구글) 및 탈퇴·재가입 구현 시 열람 필수.
layer: L2
related:
  - ../01_Core_Rules/erd_schema.md
  - ../01_Core_Rules/domain_flow.md
  - ../01_Core_Rules/test_strategy.md
---

# Auth & OAuth2 기능 명세서

## 1. 요구사항 (REQ)
- **REQ-AUTH-001**: 일반 회원가입 시 이메일·닉네임 중복 검사 후 비밀번호를 `BCrypt`로 암호화해 `users(provider=LOCAL)`에 저장한다.
- **REQ-AUTH-002**: 로그인 성공 시 `Access Token`(응답)과 `Refresh Token`(Redis 저장)을 발급하며, 재발급 시 **Refresh Token Rotation**을 적용한다.
- **REQ-AUTH-003**: **카카오(Kakao)·구글(Google) OAuth2** 로그인을 지원하며, 최초 소셜 로그인 사용자는 닉네임 설정(온보딩) 완료 후 동일 JWT 체계로 통합 발급한다.
- **REQ-AUTH-004**: 모든 인증 API는 `@RequestHeader("userId")`를 신뢰하지 않고 `@AuthenticationPrincipal`로 본인 여부를 검증한다.
- **REQ-AUTH-005**: 회원 탈퇴는 Soft Delete(`deleted_at`)로 처리하며, Partial Unique Index(`WHERE deleted_at IS NULL`)에 의해 동일 이메일/소셜 계정 재가입을 허용한다.

## 2. API / 데이터 계약
- `POST /api/auth/signup` — `{ email, password, nickname }` → `201 Created`
- `GET /api/auth/check-email?email=...` / `GET /api/auth/check-nickname?nickname=...` → `{ available: boolean }`
- `POST /api/auth/login` — `{ email, password }` → `{ accessToken, refreshToken, user }`
- `POST /api/auth/reissue` — `{ refreshToken }` → `{ accessToken, refreshToken }` (기존 RT 즉시 폐기)
- `POST /api/auth/logout` — Access Token 블랙리스트 등록 + Redis Refresh Token 삭제
- `GET /oauth2/authorization/{kakao|google}` → 콜백 처리 후 `{ accessToken, refreshToken, needsOnboarding }`
- `PATCH /api/auth/onboarding` — 소셜 최초 가입자 `{ nickname }` 설정 완료

## 3. 예외 처리 및 엣지 케이스
- 이미 사용 중인 Refresh Token이 재요청되면 탈취로 간주해 해당 유저의 Redis Refresh Token 전체를 즉시 삭제하고 `401 Unauthorized`를 반환한다.
- 탈퇴한 유저(`deleted_at IS NOT NULL`)의 로그인 시도는 `401`로 차단하고, 신규 가입 시에만 새 row로 생성한다.
- 소셜 가입자가 비밀번호 변경 API 호출 시 `400 Bad Request` (`LOCAL` 계정 전용)를 반환한다.

## 4. 검증 기준 (DoD)
- 비인증(401)·타인 접근(403) 보안 테스트 통과
- Refresh Token 재사용 탐지 단위/통합 테스트 통과
- 탈퇴 후 동일 이메일/소셜 재가입 통합 테스트 통과 (`Exit Code 0`)
