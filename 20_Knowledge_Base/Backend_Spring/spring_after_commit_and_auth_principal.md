---
description: Spring Boot 트랜잭션과 외부 API 호출 분리(AFTER_COMMIT) 및 @AuthenticationPrincipal 보안 표준.
layer: L2
related:
  - ../knowledge_moc.md
---

# Spring AFTER_COMMIT & Security Invariants

## 1. DB 트랜잭션과 외부 AI 호출 분리
- `@Transactional` 메서드 안에서 외부 HTTP/AI API를 동기 호출하면 DB 커넥션 풀이 고갈된다.
- 원본 엔티티만 커밋한 뒤 `@TransactionalEventListener(phase = TransactionPhase.AFTER_COMMIT)` 또는 `TransactionSynchronizationManager.registerSynchronization`의 `afterCommit()`에서 외부 API를 호출한다.

## 2. 인증 사용자 식별 및 Keyset 페이징
- **보안**: 컨트롤러에서 `@RequestHeader("userId")`를 신뢰하지 않고 `@AuthenticationPrincipal`로만 사용자 ID를 추출한다.
- **Keyset 페이징**: `OFFSET` 대신 커서(`lastId`) 기반 복합 조건(`(sortCol < val) OR (sortCol = val AND id < lastId)`)을 사용해 대용량에서도 일정한 응답 속도를 유지한다.
