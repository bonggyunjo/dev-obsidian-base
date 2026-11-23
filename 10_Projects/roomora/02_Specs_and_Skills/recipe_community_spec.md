---
description: 조건별 인테리어 레시피 CRUD, 복합 필터/Keyset 페이징, 좋아요/스크랩, 2-Depth 댓글 및 비동기 RAG 색인 구현 시 열람 필수.
layer: L2
related:
  - ../01_Core_Rules/erd_schema.md
  - ../01_Core_Rules/domain_flow.md
  - ./rag_search_spec.md
---

# Recipe & Community 기능 명세서

## 1. 요구사항 (REQ)
- **REQ-RCP-001**: 레시피 작성 시 구조화 메타데이터(`housing_type`, `area_pyeong`, `contract_type`, `is_no_drill`, `budget`, `style_tag`), 다중 이미지(`recipe_image`), 투입 품목(`recipe_item`)을 함께 저장한다.
- **REQ-RCP-002**: 이미지 업로드는 S3/R2 **Presigned URL** API(`POST /api/uploads/presigned-url`)를 제공하며, 로컬 개발/웹 폼에서는 외부 이미지 URL 직접 입력도 함께 지원한다.
- **REQ-RCP-003**: 목록 조회는 복합 조건 필터링 + 정렬(`LATEST`/`LIKES`/`SCRAPS`/`BUDGET_ASC`) + 전 정렬 기준 **Keyset(No-Offset) 페이징**(`lastId` 커서)을 지원한다.
- **REQ-RCP-004**: 좋아요(`recipe_like`)와 북마크(`recipe_scrap`)는 `(post_id, user_id)` Unique 제약으로 중복을 방지하고 토글 시 카운트를 동기화한다.
- **REQ-RCP-005**: 댓글(`recipe_comment`)은 2-Depth 대댓글까지 지원하며 작성자 본인만 수정·삭제할 수 있다.
- **REQ-RCP-006**: 레시피 생성·수정·삭제 커밋 후(`AFTER_COMMIT`) 비동기 이벤트로 `rag_document_chunk` 색인/갱신/삭제를 트리거한다.

## 2. API / 데이터 계약
- `POST /api/uploads/presigned-url` — `{ fileName, contentType, domain: "RECIPE" }` → `{ uploadUrl, fileUrl }`
- `POST /api/recipes` — `{ title, content, housingType, areaPyeong, contractType, isNoDrill, budget, styleTag, thumbnailUrl, images[], items[] }` → `201 Created`
- `GET /api/recipes` — Query: `housingType, isNoDrill, maxBudget, minPyeong, maxPyeong, styleTag, sort, lastId, size` → `{ items[], nextCursor, hasNext }`
- `GET /api/recipes/{id}` / `PATCH /api/recipes/{id}` / `DELETE /api/recipes/{id}`
- `POST /api/recipes/{id}/like` (토글) / `POST /api/recipes/{id}/scrap` (토글)
- `GET /api/recipes/{id}/comments` / `POST /api/recipes/{id}/comments` (`parentId` nullable) / `PATCH` / `DELETE`

## 3. 예외 처리 및 엣지 케이스
- `budget < 0` 또는 `areaPyeong <= 0` 입력 시 `@Valid`에서 `400 Bad Request` 반환.
- 타인의 레시피/댓글 수정·삭제 시도 시 `403 Forbidden` 반환.
- 비동기 RAG 임베딩 API가 실패해도 레시피 저장 트랜잭션은 롤백되지 않는다(장애 격리).
- 조회수 증가는 Redis 키(`recipe:view:{postId}:{userOrIp}`)로 단시간(예: 1시간) 중복 증가를 방지한다.

## 4. 검증 기준 (DoD)
- 복합 필터 + Keyset 페이징 정합성 및 `EXPLAIN ANALYZE` 인덱스 사용 확인
- 좋아요/스크랩 중복 요청 정합성 및 `AFTER_COMMIT` 이벤트 발행 테스트 통과
