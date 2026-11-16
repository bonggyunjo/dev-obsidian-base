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
- L1 [erd_schema.md](./01_Core_Rules/erd_schema.md) 작성 및 [project_moc.md](./00_MOC/project_moc.md) 연결

## 다음 할 일 (Next)
1. **DB 테이블 정밀 분석**: 전체 테이블 개수 확정 및 각 테이블 컬럼·관계·인덱스 구조 상세 검증 (`erd_schema.md` 고도화)
2. **GitHub CI/CD 설계 검토**: `roomora-server` / `roomora-web` 자동 빌드·테스트 파이프라인 구성 여부 및 워크플로우 확정
3. **검증 및 테스트 전략 수립**: 구현 전 도메인별 단위·통합·동시성·RAG·부하 테스트 방식 정립
4. **전체 도메인 흐름 정립**: 서비스 전반의 사용자 여정(User Flow) 및 도메인 간 데이터 흐름 명확화 후 L2 스펙(`02_Specs_and_Skills/`) 작성

## 미결정 / 주의사항
- `roomora-server` 로컬 Java/JDK 경로 확인 필요 (현재 셸 PATH 미등록)
