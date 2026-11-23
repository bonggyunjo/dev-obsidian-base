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
- 백엔드 6대 도메인(28/28 테스트) + 프론트엔드 Nuxt 3 에디토리얼 UI 리뉴얼(`ui_design_system.md` / `DEC-006`)
- **1단계 완료 — 실제 Gemini RAG 연동 검증**: `gemini-embedding-001` (1536d) + `gemini-flash-latest` 실연동 완료 (`fallback: false`, RRF 3건 인용, 응답 `21.3s`)
- **2단계 완료 — `roomora-web` 잔여 UI & 소셜 연동**: 2-Depth 대댓글, 콘테스트 출품(`/contest/submit`), 팔로워/팔로잉 모달, OAuth2 콜백·온보딩 및 `nuxi typecheck`·`nuxt build` 통과
- **3단계 완료 — 시드 60건 확충 & 백엔드 보완**: 가이드 10 + 레시피 45 + 콘테스트 5(총 60건) 시딩, `CONTEST` RAG 색인 추가, `RecipeService` 비-`LATEST`(`LIKES`/`SCRAPS`/`BUDGET_ASC`) 키셋 페이징 보완, 임베딩 폴백 안전장치 추가 및 `gradle test` 통과
- 공용 `AGENTS.md`에 **비용·결제 안전 가드레일** 규칙 추가

## 다음 할 일 (Next)
1. **L2 스펙 ↔ 구현 동기화**: `02_Specs_and_Skills/*_spec.md`와 실제 코드 전수 대조(누락 구현 or 스펙 역반영)
2. **성능 수치화 (`EXPLAIN ANALYZE` + `k6`)**: Keyset vs Offset, Redis ZSET vs DB, RAG 캐시 Hit/Miss TPS·P95 측정 및 문서화

## 미결정 / 주의사항
- `roomora-server` 호스트 실행 시 `5432` 포트 충돌 주의 (`ERR-001`)
