---
description: 아키텍처 및 기획 주요 의사결정 기록 (ADR). 필요 시 DEC-nnn ID로 발췌 열람.
layer: L3
---

# Architecture Decision Records (ADR)

### DEC-001: YOLOv5 · 3D 배치 · 실시간 채팅 제외 및 AI RAG 통합 검색 피벗
- **날짜**: 2026-11-14
- **상태**: Accepted
- **배경 (Context)**: 기존 캡스톤의 YOLO 이미지 추론(높은 RAM/CPU 비용, 120초 동기 블로킹)과 3D 배치, 1:1 채팅은 개인 포트폴리오의 저비용 실제 배포 및 초기 사용자 환경에 부적합함.
- **결정 (Decision)**: 이미지 AI·3D·채팅을 제거하고, 조건별 레시피 + 주간 콘테스트(Redis ZSET) + AI RAG 통합 검색 + AI 체크리스트 중심으로 재정의함.
- **이유 및 트레이드오프 (Rationale)**: 텍스트 기반 Hybrid RAG는 운영비를 거의 0원 수준으로 통제하면서도 비동기 파이프라인, 캐싱, 장애 격리 등 백엔드 엔지니어링 깊이를 증명할 수 있음.

### DEC-002: 공용 옵시디언 볼트 + 개인 GitHub 레포 3종 분리 구조
- **날짜**: 2026-11-14
- **상태**: Accepted
- **배경 (Context)**: `server`와 `web` 레포를 분리하면서 문서와 AI 에이전트 컨텍스트를 일관되게 공유해야 함.
- **결정 (Decision)**: 공용 볼트(`dev-obsidian-base`, Private) 1개와 개인 계정 아래 `server` / `web` 레포 2개(Public)를 분리 운영함.
- **이유 및 트레이드오프 (Rationale)**: 원본 팀 Org 레포는 Before 레퍼런스로 보존하고, 개인 레포로 1인 설계·구축 기여도를 명확히 분리함.
