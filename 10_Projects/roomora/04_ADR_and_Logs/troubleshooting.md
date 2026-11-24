---
description: 개발 및 운영 중 발생한 트러블슈팅 오답노트 (ERR-nnn).
layer: L3
---

# Troubleshooting Log

### ERR-001: Windows 로컬 PostgreSQL 5432 포트 충돌로 인한 Flyway 인증 실패
- **날짜**: 2026-11-18
- **증상**: Docker `roomora-postgres`가 기동 중인데도 `host.docker.internal:5432` 연결 시 `role "roomora" does not exist` (SQL State `28000`) 발생
- **원인 (Root Cause)**: Windows 호스트에 이미 설치된 로컬 PostgreSQL 서비스가 `5432` 포트를 점유하고 있어 Docker 컨테이너가 아닌 호스트 DB로 라우팅됨
- **해결 방법**: 테스트 실행 시 Docker 내부 네트워크(`--network roomora-server_default`)로 `roomora-postgres:5432`에 직접 연결해 해결
- **재발 방지**: 추후 호스트(IntelliJ)에서 직접 실행 시 `docker-compose.yml` 호스트 포트를 `5433:5432`로 매핑하거나 Windows PostgreSQL 서비스를 중지한다.

## ERR-002: JPA `@Modifying(clearAutomatically = true)` 사용 시 `delete()` 미반영 버그
- **증상**: 레시피 좋아요 토글(`0 → 1 → 0`) 후 다시 좋아요를 눌러도 계속 `active=false, count=0`만 반환됨.
- **원인**: `RecipeService.toggleLike`에서 `recipeLikeRepository.delete(...)` 호출 후 `@Modifying(clearAutomatically = true)`만 설정된 JPQL `UPDATE`(카운트 감소)가 실행되면서, 대기 중이던 `DELETE`가 DB에 flush되기 전에 영속성 컨텍스트가 초기화됨.
- **해결**: `RecipePostRepository`, `ContestPostRepository`, `UserRepository`의 모든 `@Modifying` 쿼리에 `flushAutomatically = true`를 함께 지정.
- **재발 방지**: Spring Data JPA `@Modifying(clearAutomatically = true)` 사용 시 반드시 `flushAutomatically = true`를 함께 선언한다.
