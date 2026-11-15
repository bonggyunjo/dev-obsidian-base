---
description: 구현 전 plan.md가 아키텍처 규칙(L1/DEC)을 위반하지 않는지 심사하는 Gate 1 감사 체크리스트.
layer: L0
---

# Gate 1: Plan Auditor Checklist (Read-Only)

> **권한 제약**: 감사 에이전트는 코드 및 문서를 직접 수정하지 않고 **PASS / REJECT** 판정과 근거만 반환한다.

## 1. 사전 로드 확인
- [ ] 관련 L1(`01_Core_Rules/`) 및 L2(`02_Specs_and_Skills/`) 문서를 읽고 `plan.md`를 작성했는가?
- [ ] 기존 `DEC-nnn` 결정사항과 충돌하는 설계는 없는가?

## 2. 아키텍처 & 보안 체크
- [ ] 인증/인가 검증을 헤더나 클라이언트 입력값에 의존하지 않는가?
- [ ] 외부 API(LLM/임베딩 등) 호출이 핵심 DB 트랜잭션 안에 동기 블로킹으로 묶여 있지 않은가?
- [ ] 대량 목록 조회에 페이징(No-Offset/Offset) 및 인덱스 고려가 포함되어 있는가?

## 3. 범위 & 검증 체크
- [ ] `Do Not Touch` 구역(기존 마이그레이션, `.env` 등)을 건드리지 않는가?
- [ ] 전체 빌드가 아닌 **Narrowest 테스트 명령**이 명시되어 있는가?
