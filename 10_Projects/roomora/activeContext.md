---
description: Roomora 프로젝트 현재 작업 상태 인수인계서 (50줄 이내).
layer: L0.5
related:
  - ./00_MOC/project_moc.md
---

# Active Context — Roomora

## 현재 단계
- **1차 풀스택 구축 완료 → 실연동·고도화·수치 검증 5단계 진입**

## 방금 완료한 일
- 백엔드 6대 도메인(28/28 테스트) + 프론트엔드 Nuxt 3 에디토리얼 UI 리뉴얼(`ui_design_system.md` / `DEC-006`) 및 `nuxi typecheck`·`nuxt build` 통과

## 다음 할 일 (Next)
1. **실제 `AI_API_KEY` RAG 연동**: OpenAI/Gemini 호환 키 연결 후 실제 임베딩·Structured JSON 응답 품질 및 Latency 실측
2. **`roomora-web` 잔여 UI & 소셜 연동**: 대댓글 달기, 콘테스트 출품(`/contest/submit`), 팔로워/팔로잉 모달, 카카오·구글 OAuth2 실연동 및 E2E 점검
3. **시드 데이터 50건+ 확충 & 백엔드 보완**: 현실 레시피/가이드 50건 이상 적재, `CONTEST` RAG 색인 추가, `RecipeService` 비-LATEST 페이징 보완
4. **L2 스펙 ↔ 구현 동기화**: `02_Specs_and_Skills/*_spec.md`와 실제 코드 전수 대조(누락 구현 or 스펙 역반영)
5. **성능 수치화 (`EXPLAIN ANALYZE` + `k6`)**: Keyset vs Offset, Redis ZSET vs DB, RAG 캐시 Hit/Miss TPS·P95 측정 및 문서화

## 미결정 / 주의사항
- `roomora-server` 호스트 실행 시 `5432` 포트 충돌 주의 (`ERR-001`)
