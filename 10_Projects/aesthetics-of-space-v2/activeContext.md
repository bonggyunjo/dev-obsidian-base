---
description: Roomora (The-Aesthetics-Of-Space_v2) 프로젝트 현재 작업 상태 인수인계서 (50줄 이내).
layer: L0.5
related:
  - ./00_MOC/project_moc.md
---

# Active Context — Roomora (aesthetics-of-space-v2)

## 현재 단계
- **설계 단계 (Architect)**: L1 DB/ERD 설계 완료 → L2 도메인 스펙 작성 차례

## 방금 완료한 일
- 서비스/레포명 **Roomora** 확정 (`roomora-server`, `roomora-web` 초기 레포 구축 및 GitHub 푸시 완료)
- 핵심 정책 확정: `DEC-003` (PostgreSQL 16 + `pgvector`), `DEC-004` (콘테스트 회차당 1인 1표)
- L1 [erd_schema.md](./01_Core_Rules/erd_schema.md) 작성 및 [project_moc.md](./00_MOC/project_moc.md) 연결

## 다음 할 일 (Next)
1. 도메인별 L2 스펙(`02_Specs_and_Skills/`) 작성:
   - `auth_oauth_spec.md`, `recipe_community_spec.md`, `contest_ranking_spec.md`, `rag_search_spec.md`, `checklist_spec.md`, `mypage_social_spec.md`
2. 스펙 확정 후 `roomora-server` Flyway V1 DDL 및 도메인 순차 구현

## 미결정 / 주의사항
- `roomora-server` 로컬 Java/JDK 경로 확인 필요 (현재 셸 PATH 미등록)
