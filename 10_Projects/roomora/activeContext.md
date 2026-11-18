---
description: Roomora 프로젝트 현재 작업 상태 인수인계서 (50줄 이내).
layer: L0.5
related:
  - ./00_MOC/project_moc.md
---

# Active Context — Roomora

## 현재 단계
- **설계 완료 → 구현 1단계 진입**: L1(4종) + L2(6대 도메인 스펙) 작성 완료

## 방금 완료한 일
- **L2 도메인 스펙 6종 작성 완료** (`02_Specs_and_Skills/`):
  - `auth_oauth_spec`, `recipe_community_spec`, `contest_ranking_spec`, `rag_search_spec`, `checklist_spec`, `mypage_social_spec`
- [project_moc.md](./00_MOC/project_moc.md)에 L2 스펙 전체 라우팅 연결

## 다음 할 일 (Next)
1. **`roomora-server` 인프라 & Flyway V1 DDL 구축**:
   - 로컬 Docker Compose (`pgvector/pgvector:pg16` + `redis:7`) 구성
   - `erd_schema.md` 기반 `V1__init.sql` (18개 테이블 + 인덱스) 작성
2. **도메인 순차 구현 (Plan → Gate 1 → Code → QA Agent → Gate 2)**:
   - 1순위: Auth & OAuth2 (`auth_oauth_spec.md`)

## 미결정 / 주의사항
- `roomora-server` 로컬 Java 17/JDK 및 Docker 실행 환경 확인 필요
