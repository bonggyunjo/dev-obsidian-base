---
description: 작업 종료 시 문서 동기화 및 다음 세션 인수인계를 위한 Self-Updating SOP.
layer: L0
---

# Session Handoff SOP (작업 종료 루프)

기능 1개 또는 마일스톤을 마치고 새 대화 세션을 열기 전, 반드시 아래 3단계를 순서대로 수행한다.

1. **L2 스펙 동기화**: 구현 중 변경된 API 시그니처나 정책을 해당 프로젝트의 `02_Specs_and_Skills/*_spec.md`에 반영한다.
2. **L3 로그 기록 (Append)**:
   - 아키텍처·기술 선택이 있었다면 `04_ADR_and_Logs/decisions.md`에 `DEC-nnn`으로 추가한다.
   - 해결한 버그나 시행착오가 있었다면 `04_ADR_and_Logs/troubleshooting.md`에 `ERR-nnn`으로 추가한다.
3. **L0.5 `activeContext.md` 덮어쓰기 (Overwrite)**:
   - 과거 내역은 비우고 **현재 단계 / 방금 완료한 일 / 다음 할 일 / 주의사항**만 **50줄 이내**로 덮어쓴다.
   - 이후 새 대화 세션에서는 `activeContext.md`만 읽고 깨끗한 컨텍스트로 이어받는다.
