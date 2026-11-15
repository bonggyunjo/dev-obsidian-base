# dev-obsidian-base

여러 개발 프로젝트의 아키텍처, 도메인 스펙, 의사결정(ADR), 트러블슈팅을 관리하는 **공용 옵시디언(Obsidian) 지식 볼트**입니다.

## 폴더 구조

```txt
dev-obsidian-base/
├── AGENTS.md                  # [L0] 볼트 문서 작성 규칙 & AI 에이전트 표준
├── 00_Inbox/                  # 임시 메모 및 아이디어
├── 10_Projects/               # 프로젝트별 독립 문서 공간
│   └── aesthetics-of-space-v2/
├── 20_Knowledge_Base/         # 프로젝트 공통 기술 노트 (PostgreSQL, Redis, Spring AI 등)
└── 90_Templates/              # 스펙 / ADR / 트러블슈팅 / activeContext 템플릿
```

## 옵시디언 필수 설정 (최초 1회)
- **설정 → 파일 및 링크 → 새 링크 형식**: `파일에 대한 상대 경로`
- **설정 → 파일 및 링크 → Wikilinks 사용**: **OFF** (`[제목](./파일.md)` 표준 마크다운 링크 사용)
