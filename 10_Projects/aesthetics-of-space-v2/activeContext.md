---
description: The-Aesthetics-Of-Space_v2 프로젝트 현재 작업 상태 인수인계서 (50줄 이내).
layer: L0.5
related:
  - ./00_MOC/project_moc.md
---

# Active Context — aesthetics-of-space-v2

## 현재 단계
- **설계 및 기획 단계** (노션 `The-Aesthetics-Of-Space_v2` 01~06 챕터 정리 완료)

## 방금 완료한 일
- 레거시(`The-Aesthetics-Of-Space-Project`) 코드 진단 및 기능 취사선택 완료
- 기능 확정: 조건별 레시피 + 주간 콘테스트(Redis ZSET) + AI RAG 통합 검색 + AI 체크리스트 + OAuth2(카카오/구글) + 팔로우/좋아요/스크랩 (YOLO·3D·채팅 제외)
- 공용 옵시디언 볼트(`dev-obsidian-base`) 초기 구조 구축

## 다음 할 일 (Next)
1. To-Be ERD 확정 (`01_Core_Rules/erd_schema.md`)
2. 개인 GitHub 레포 생성 (`aesthetics-of-space-server`, `aesthetics-of-space-web`)
3. 도메인별 L2 스펙(`02_Specs_and_Skills/`) 상세 작성

## 미결정 / 주의사항
- DB 최종 확정 필요: PostgreSQL 16 + `pgvector` (권장) vs MariaDB + 별도 Vector DB
