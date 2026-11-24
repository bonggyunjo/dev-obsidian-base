---
description: Roomora 프로젝트 현재 작업 상태 인수인계서 (50줄 이내).
layer: L0.5
related:
  - ./00_MOC/project_moc.md
---

# Active Context — Roomora

## 현재 단계
- **5단계 로드맵 전체 완료** (`1차 풀스택 구축 → Gemini 실연동 → 프론트 잔여 UI → 시드 60건/백엔드 보완 → 스펙 동기화 → 성능 수치화`)

## 방금 완료한 일
- **1~4단계 완료**: Gemini RAG 실연동, `roomora-web` 잔여 UI·OAuth2 콜백, 시드 60건 + `CONTEST` RAG 색인 + 전 정렬 키셋 페이징, L2 스펙 6종 역반영
- **5단계 완료 — 성능 수치화 (`EXPLAIN ANALYZE` + `k6`)**: Keyset(0.060ms) vs Offset(12.021ms), HNSW(2.920ms), `k6` 68,697건 0% 에러 (Recipes P95 24.16ms, Contest ZSET P95 18.21ms, RAG Cache Hit P95 9.53ms) → [performance_benchmarks.md](./04_ADR_and_Logs/performance_benchmarks.md) 기록

## 다음 할 일 (Next)
1. **브라우저 실화면 E2E 점검 & UI/UX 다듬기**: `roomora-server` + `roomora-web` 동시 기동 후 레시피·대댓글·콘테스트 출품/투표·AI 검색→체크리스트 저장·팔로우 모달 실사용 흐름 및 반응형/마이크로 UX 보완
2. **외부 Provider 실환경 연동**: 카카오·구글 OAuth2 실키(`.env`) 등록 후 소셜 로그인→온보딩 E2E 검증, S3/R2 Presigned URL 이미지 직접 업로드 연동

## 미결정 / 주의사항
- `roomora-server` 호스트 실행 시 `5432` 포트 충돌 주의 (`ERR-001`)
