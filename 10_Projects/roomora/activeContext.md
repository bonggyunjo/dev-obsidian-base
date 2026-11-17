---
description: Roomora (The-Aesthetics-Of-Space_v2) 프로젝트 현재 작업 상태 인수인계서 (50줄 이내).
layer: L0.5
related:
  - ./00_MOC/project_moc.md
---

# Active Context — Roomora

## 현재 단계
- **설계 단계 (Architect)**: L1 DB/ERD 설계 완료 → L2 도메인 스펙 작성 차례

## 방금 완료한 일
- 서비스/레포명 **Roomora** 확정 (`roomora-server`, `roomora-web` 초기 레포 구축 및 GitHub 푸시 완료)
- 핵심 정책 확정: `DEC-003` (PostgreSQL 16 + `pgvector`), `DEC-004` (콘테스트 회차당 1인 1표)
- **DB 테이블 정밀 분석 완료**: [erd_schema.md](./01_Core_Rules/erd_schema.md) 총 18개 테이블 확정 (`contest_image` 추가, Partial Unique, `pg_trgm` GIN 인덱스 보완)
- **GitHub CI/CD 설계 확정**: `DEC-005` + `roomora-server` / `roomora-web` CI 구축 완료
- **검증 및 테스트 전략 수립 완료**: [test_strategy.md](./01_Core_Rules/test_strategy.md) 및 `qa_test_agent.md` 생성 (5단계 검증 프로세스)

## 다음 할 일 (Next)
1. **전체 도메인 흐름 정립** *(진행 중)*: 서비스 전반의 사용자 여정(User Flow) 및 도메인 간 데이터 흐름 명확화 후 L2 스펙(`02_Specs_and_Skills/`) 작성

## 미결정 / 주의사항
- `roomora-server` 로컬 Java/JDK 경로 확인 필요 (현재 셸 PATH 미등록)
