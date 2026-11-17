---
description: Roomora (The-Aesthetics-Of-Space_v2) 프로젝트 현재 작업 상태 인수인계서 (50줄 이내).
layer: L0.5
related:
  - ./00_MOC/project_moc.md
---

# Active Context — Roomora

## 현재 단계
- **설계 단계 (Architect)**: L1 아키텍처·ERD·CI·테스트전략·도메인흐름 완료 → **L2 도메인 스펙 작성 차례**

## 방금 완료한 일
- 서비스/레포명 **Roomora** 확정 (`roomora-server`, `roomora-web` 초기 레포 구축 및 GitHub 푸시 완료)
- DB·CI·테스트 전략 확정: [erd_schema.md](./01_Core_Rules/erd_schema.md) (18개 테이블), `DEC-005` (CI 구축), [test_strategy.md](./01_Core_Rules/test_strategy.md)
- **전체 도메인 흐름 정립 완료**: [domain_flow.md](./01_Core_Rules/domain_flow.md) (핵심 순환 루프 및 5대 도메인 Flow 정리)

## 다음 할 일 (Next)
1. **L2 도메인별 상세 스펙(`02_Specs_and_Skills/`) 작성**:
   - `auth_oauth_spec.md`, `recipe_community_spec.md`, `contest_ranking_spec.md`, `rag_search_spec.md`, `checklist_spec.md`, `mypage_social_spec.md`
2. 스펙 확정 후 `roomora-server` Flyway V1 DDL 및 도메인 순차 구현 (구현 → QA Agent → Gate 2 루프)

## 미결정 / 주의사항
- `roomora-server` 로컬 Java/JDK 경로 확인 필요 (현재 셸 PATH 미등록)
