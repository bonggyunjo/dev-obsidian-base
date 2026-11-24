---
description: Redis Refresh Token Rotation 탈취 탐지 및 ZSET 실시간 랭킹 설계 패턴.
layer: L2
related:
  - ../knowledge_moc.md
---

# Redis RT Rotation & ZSET Ranking

## 1. Refresh Token Rotation (탈취 탐지)
- 로그인/재발급 시 Refresh Token을 Redis에 저장하고, 재발급 요청이 오면 기존 RT를 즉시 폐기한 뒤 새 RT를 발급한다.
- 이미 폐기된 RT가 다시 들어오면 토큰 탈취로 간주해 해당 사용자의 Redis 세션 전체를 삭제하고 `401`을 반환한다.

## 2. Redis ZSET 실시간 랭킹 + 동시성 제어
- 투표 정합성은 RDB `UNIQUE (round_id, user_id)` 제약으로 100스레드 동시 요청에서도 1표만 허용한다.
- 실시간 Top N 조회는 `ORDER BY vote_count DESC` 대신 Redis ZSET(`ZREVRANGE`)을 사용해 조회 부하를 줄이고, 회차 마감 시에만 RDB `final_rank`에 확정 저장한다.
