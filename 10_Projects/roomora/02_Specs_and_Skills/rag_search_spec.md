---
description: AI RAG 통합 검색 파이프라인, 비동기 청킹/임베딩, Hybrid Search(Pre-filtering + pg_trgm + HNSW) 및 Resilience4j Fallback 구현 시 열람 필수.
layer: L2
related:
  - ../01_Core_Rules/erd_schema.md
  - ../01_Core_Rules/domain_flow.md
  - ./checklist_spec.md
---

# AI RAG 통합 검색 기능 명세서

## 1. 요구사항 (REQ)
- **REQ-RAG-001**: 레시피·가이드·콘테스트 우수작 커밋 후(`AFTER_COMMIT`) 메타데이터 Prefix(`[주거형태 | 평수 | 계약 | 무타공 | 예산]`)를 포함한 청크를 생성해 임베딩 후 `rag_document_chunk`에 멱등 Upsert(`UNIQUE(source_type, source_id, chunk_index)`)한다.
- **REQ-RAG-002**: 검색 요청 시 Redis Rate Limit(일일 호출 제한) 및 질의 캐시를 먼저 확인한다.
- **REQ-RAG-003**: 자연어 질의에서 정형 조건(예산/평수/무타공/주거형태)과 의미 검색어를 분리(Query Parsing)한다.
- **REQ-RAG-004**: PostgreSQL 단일 DB에서 정형 조건 **Pre-filtering** 후 `pg_trgm` 키워드 + `pgvector` HNSW 코사인 유사도를 결합(Hybrid Search + RRF)해 Top-K 청크를 조회한다.
- **REQ-RAG-005**: 검색된 `<document>` 컨텍스트만 기반으로 Structured JSON(맞춤 처방, 예산 배분표, 근거 Citation 목록, 체크리스트 초안, 주의 경고)을 생성한다.
- **REQ-RAG-006**: 외부 LLM/임베딩 API 장애·타임아웃 시 **Resilience4j Circuit Breaker Fallback**으로 RDB 조건 필터 + 키워드 검색 결과를 `200 OK` (`fallback: true`)로 반환한다.

## 2. API / 데이터 계약
- `POST /api/search/rag`
  - Request: `{ query: string, filters?: { housingType, maxBudget, isNoDrill, maxPyeong } }`
  - Response:
    ```json
    {
      "fallback": false,
      "summary": "맞춤 처방 요약...",
      "warnings": ["북향은 3000K 전구색 권장", "..."],
      "budgetBreakdown": [{ "category": "조명", "amount": 80000 }],
      "citations": [{ "sourceType": "RECIPE", "sourceId": 12, "title": "...", "thumbnailUrl": "...", "budget": 450000 }],
      "checklistDraft": [{ "stepOrder": 1, "category": "조명", "content": "...", "estimatedCost": 80000 }]
    }
    ```
- `POST /api/admin/rag/reindex` — 가이드/시드 데이터 일괄 재임베딩 (ADMIN 전용)

## 3. 예외 처리 및 엣지 케이스
- 조건에 정확히 일치하는 레시피가 0건일 경우, 필터를 단계적으로 완화해 유사 사례를 찾되 응답에 완화 여부를 명시한다.
- 게시글 본문의 Prompt Injection 시도를 막기 위해 컨텍스트를 `<document id="...">...</document>` 태그로 격리한다.
- Rate Limit 초과 시 `429 Too Many Requests`를 반환하며 일반 조건 검색(`/api/recipes`) 이용을 안내한다.

## 4. 검증 기준 (DoD)
- Mock `ChatClient`/`EmbeddingModel` 기반 Hybrid Search Pre-filtering 정확도 테스트 통과
- 재임베딩 재시도 시 청크 중복 미발생(멱등성) 테스트 통과
- AI 타임아웃/예외 모의 시 `fallback: true` 200 OK 응답 검증 통과
