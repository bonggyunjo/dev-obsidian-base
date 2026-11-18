---
description: AI RAG 검색 결과의 1-Click 체크리스트 저장, 항목 완료 체크/메모 및 진행률 계산 구현 시 열람 필수.
layer: L2
related:
  - ../01_Core_Rules/erd_schema.md
  - ./rag_search_spec.md
  - ./mypage_social_spec.md
---

# AI Checklist 기능 명세서

## 1. 요구사항 (REQ)
- **REQ-CHK-001**: 로그인한 사용자는 RAG 검색 결과(`checklistDraft`)를 1-Click으로 본인 계정의 `ai_checklist` + `ai_checklist_item`에 저장할 수 있다.
- **REQ-CHK-002**: 마이페이지에서 내 체크리스트 목록과 각 체크리스트의 전체 항목 수 대비 완료 항목 수(진행률 `%`)를 조회할 수 있다.
- **REQ-CHK-003**: 사용자는 각 항목의 완료 여부(`is_checked`) 토글, 메모(`memo`) 수정, 항목 추가·삭제를 수행할 수 있다.
- **REQ-CHK-004**: 체크리스트 조회·수정·삭제는 소유자 본인(`checklist.user_id == currentUser.id`)만 가능하다.

## 2. API / 데이터 계약
- `POST /api/checklists` — `{ title, querySummary, targetBudget, housingType, isNoDrill, items: [{ stepOrder, category, content, estimatedCost }] }` → `201 Created`
- `GET /api/checklists` — 내 체크리스트 목록 + 진행률(`completedCount`, `totalCount`, `progressPercent`)
- `GET /api/checklists/{id}` — 체크리스트 상세 및 `step_order ASC` 정렬된 항목 목록
- `PATCH /api/checklists/{id}/items/{itemId}` — `{ isChecked?, memo?, content?, estimatedCost? }`
- `DELETE /api/checklists/{id}`

## 3. 예외 처리 및 엣지 케이스
- 타인의 체크리스트 ID로 조회·수정·삭제 요청 시 `403 Forbidden` 반환.
- 존재하지 않는 체크리스트/항목 ID 요청 시 `404 Not Found` 반환.
- 항목이 0개일 때 진행률 계산에서 Zero-Division 없이 `0%`를 반환한다.

## 4. 검증 기준 (DoD)
- 1-Click 저장 후 항목 순서(`step_order`) 및 진행률 계산 정확성 테스트 통과
- 타인 체크리스트 수정 시도 `403 Forbidden` 검증 통과
