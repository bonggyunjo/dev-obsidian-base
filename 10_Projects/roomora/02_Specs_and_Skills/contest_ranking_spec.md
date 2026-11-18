---
description: 주간 콘테스트 회차 관리, 출품작 CRUD, 회차당 1인 1표 동시성 제어, Redis ZSET 실시간 랭킹 및 마감 배치 구현 시 열람 필수.
layer: L2
related:
  - ../01_Core_Rules/erd_schema.md
  - ../01_Core_Rules/domain_flow.md
  - ../04_ADR_and_Logs/decisions.md
---

# Contest & Ranking 기능 명세서

## 1. 요구사항 (REQ)
- **REQ-CNT-001**: 콘테스트 회차(`contest_round`)는 `UPCOMING` / `ACTIVE` / `CLOSED` 상태를 가지며 진행 중 회차와 지난 회차(명예의 전당)를 조회할 수 있다.
- **REQ-CNT-002**: 사용자는 `ACTIVE` 회차당 **1개의 출품작**(`UNIQUE (round_id, user_id) WHERE deleted_at IS NULL`)만 등록할 수 있으며 다중 이미지(`contest_image`)를 첨부한다.
- **REQ-CNT-003**: **`DEC-004` 회차당 1인 1표** 정책에 따라 `contest_vote`에 `UNIQUE (round_id, user_id)`를 적용하고 동시 요청 정합성을 보장한다.
- **REQ-CNT-004**: 실시간 랭킹 Top N 조회는 DB `ORDER BY` 대신 **Redis ZSET** (`contest:ranking:{roundId}`)을 사용한다.
- **REQ-CNT-005**: 투표와 별개로 출품작 북마크(`contest_scrap`) 및 2-Depth 댓글(`contest_comment`)을 지원한다.
- **REQ-CNT-006**: 회차 종료 시각 도달 시 스케줄러가 상태를 `CLOSED`로 전환하고 Redis ZSET 최종 순위를 `contest_post.final_rank`에 확정 저장한다.

## 2. API / 데이터 계약
- `GET /api/contests/rounds/active` / `GET /api/contests/rounds/{roundId}/winners`
- `GET /api/contests/rounds/{roundId}/ranking?limit=10` — Redis ZSET Top N 반환
- `POST /api/contests/rounds/{roundId}/posts` — `{ title, content, thumbnailUrl, images[] }` → `201 Created`
- `GET /api/contests/posts/{postId}` / `PATCH` / `DELETE`
- `POST /api/contests/posts/{postId}/vote` — 회차당 1표 투표 (이미 다른 작품에 투표했거나 동일 작품 재투표 시 정책에 따라 `409 Conflict` 반환)
- `POST /api/contests/posts/{postId}/scrap` (북마크 토글)
- `GET / POST / PATCH / DELETE /api/contests/posts/{postId}/comments`

## 3. 예외 처리 및 엣지 케이스
- `ACTIVE`가 아닌 회차에 출품·투표 요청 시 `400 Bad Request` 반환.
- 동일 유저가 동시에 100개의 투표 요청을 보내도 DB Unique 제약 + 동시성 제어로 정확히 1표만 반영되고 `vote_count`와 Redis ZSET 점수가 일치해야 한다.
- 본인이 출품한 작품에 본인 투표 허용/차단 정책: 공정성을 위해 본인 작품 자가 투표는 `400 Bad Request`로 차단한다.

## 4. 검증 기준 (DoD)
- `ExecutorService` + `CountDownLatch` **100스레드 동시 투표 정합성 테스트** 통과
- 마감 배치 실행 시 Redis ZSET → RDB `final_rank` 영구 저장 검증 통과
