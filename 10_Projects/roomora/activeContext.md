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
- **1~5단계 완료**: Gemini RAG 실연동, `roomora-web` 잔여 UI·OAuth2 콜백, 시드 60건·`CONTEST` RAG 색인·전 정렬 키셋 페이징, L2 스펙 6종 동기화, `EXPLAIN ANALYZE`·`k6` 수치화([performance_benchmarks.md](./04_ADR_and_Logs/performance_benchmarks.md))
- **브라우저 E2E 점검 & UI/UX 다듬기 완료**: 데스크톱(1280px)·모바일(375px) 실화면 전수 점검, JPA `@Modifying(flushAutomatically = true)` 좋아요/스크랩 재토글 버그 수정([troubleshooting.md](./04_ADR_and_Logs/troubleshooting.md) `ERR-002`), 모바일 헤더·히어로 줄바꿈/한국어 라벨·본문 `##` 포맷팅 개선 및 `gradle test`·`nuxt build` 통과

## 다음 할 일 (Next)
1. **외부 Provider 실환경 연동**: 카카오·구글 OAuth2 실키(`.env`) 등록 후 소셜 로그인→온보딩 E2E 검증, S3/R2 Presigned URL 이미지 직접 업로드 연동

## 미결정 / 주의사항
- `roomora-server` 호스트 실행 시 `5432` 포트 충돌 주의 (`ERR-001`)
