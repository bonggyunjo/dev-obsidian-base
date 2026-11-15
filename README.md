# dev-obsidian-base

여러 개발 프로젝트의 **아키텍처 설계, 도메인 스펙, 의사결정(ADR), 트러블슈팅**을 하나로 관리하고, 분리된 코드 레포지토리(`server` / `web`)의 AI 코딩 에이전트가 문맥을 일관되게 참조·갱신할 수 있도록 만든 **공용 옵시디언(Obsidian) 지식 볼트**입니다.

---

## 1. 디렉토리 구조 및 계층 역할 (L0 ~ L3)

```txt
dev-obsidian-base/
├── AGENTS.md                        # [L0] 볼트 전역 문서 작성 규칙 & AI 에이전트 표준 (SSOT)
├── README.md                        # 볼트 전체 안내 및 사용 가이드
├── 00_Inbox/                        # 정제 전 임시 메모, 아이디어 스케치
├── 10_Projects/                     # 프로젝트별 독립 문서 공간
│   └── aesthetics-of-space-v2/      # 현재 진행 중인 프로젝트
│       ├── activeContext.md         # [L0.5] 현재 작업 상태 & 다음 할 일 (50줄 이내 덮어쓰기)
│       ├── 00_MOC/                  # [L0] 프로젝트 문서 목차 (Map of Content)
│       ├── 01_Core_Rules/           # [L1] 시스템 아키텍처, ERD, 코딩 표준
│       ├── 02_Specs_and_Skills/     # [L2] 도메인별 상세 기능 스펙 (REQ-nnn)
│       └── 04_ADR_and_Logs/         # [L3] 아키텍처 결정 기록(DEC-nnn) & 트러블슈팅(ERR-nnn)
├── 20_Knowledge_Base/               # 프로젝트 공통 기술 노트 (카테고리별 분리)
│   ├── knowledge_moc.md             # 기술 지식 베이스 목차
│   ├── Backend_Spring/
│   ├── DB_Redis/
│   ├── AI_RAG/
│   └── Frontend_Nuxt/
├── 30_Agent_Playbooks/              # 공용 AI 에이전트 감사 체크리스트 & 세션 SOP
│   ├── gate1_plan_auditor.md
│   ├── gate2_code_reviewer.md
│   └── session_handoff_sop.md
└── 90_Templates/                    # 새 문서 작성용 표준 템플릿 5종
    ├── active_context_template.md
    ├── plan_template.md
    ├── spec_template.md
    ├── decision_template.md
    └── troubleshooting_template.md
```

| 계층 | 역할 | 에이전트 열람 및 갱신 방식 |
| :--- | :--- | :--- |
| **L0 (`AGENTS.md`, `00_MOC/`)** | 볼트 규칙 및 문서 라우팅 지도 | 세션 시작 시 진입점으로 열람 |
| **L0.5 (`activeContext.md`)** | 현재 진행 상태 단기 기억 | 세션 시작 시 열람, 종료 시 **50줄 이내로 덮어쓰기** |
| **L1 (`01_Core_Rules/`)** | 전역 아키텍처 및 DB 스키마 | 구조 설계·변경 시 열람 |
| **L2 (`02_Specs_and_Skills/`)** | 도메인별 기능 스펙 (`REQ-nnn`) | 해당 기능 구현 시 선택 열람 |
| **L3 (`04_ADR_and_Logs/`)** | 결정 이력(`DEC-nnn`) · 오류 기록(`ERR-nnn`) | 디버깅·의사결정 시 조건부 열람 및 누적(Append) |

---

## 2. 관리 중인 프로젝트

- **[aesthetics-of-space-v2](./10_Projects/aesthetics-of-space-v2/00_MOC/project_moc.md)**: 학부 캡스톤(`The-Aesthetics-Of-Space-Project`)을 재설계한 **조건 기반 인테리어 레시피 · 주간 콘테스트 · AI RAG 통합 검색 플랫폼**

---

## 3. 코드 레포지토리 연동 방식

백엔드(`server`)와 프론트엔드(`web`) 레포지토리를 분리 운영하는 경우, 아래와 같이 **같은 상위 폴더 아래에 나란히 클론**해 상대 경로로 연결합니다.

```txt
workspace/
├── dev-obsidian-base/                 # 공용 문서 볼트 (Private)
├── aesthetics-of-space-server/        # Spring Boot 백엔드 (Public)
└── aesthetics-of-space-web/           # Nuxt 3 프론트엔드 (Public)
```

- 각 코드 레포의 `AGENTS.md`에서 `../dev-obsidian-base/10_Projects/<project>/` 경로를 참조하도록 지정하면, AI 에이전트가 다른 프로젝트 문서를 섞지 않고 현재 프로젝트 스펙만 정확히 읽고 갱신합니다.

---

## 4. 옵시디언 앱 초기 설정 & 작성 표준

### 최초 1회 필수 설정
1. 옵시디언 실행 → **폴더를 보관함으로 열기** → `dev-obsidian-base` 선택
2. `설정(톱니바퀴) → 파일 및 링크`에서 아래 두 옵션 적용:
   - **새 링크 형식**: `파일에 대한 상대 경로`
   - **Wikilinks 사용**: **OFF (끄기)** → `[[문서]]` 대신 `[문서](./file.md)` 표준 마크다운 링크 생성

### 문서 작성 5대 규칙
1. **최소 YAML Frontmatter**: `description`(언제 읽을지), `layer`, `related`만 기재
2. **상대 경로 링크 사용**: GitHub 웹 뷰어 및 에이전트 파일 읽기 도구 호환성 보장
3. **고유 ID 부여**: `REQ-001`, `DEC-001`, `ERR-001` 번호를 붙여 `grep -n`으로 필요한 부분만 발췌 가능하게 유지
4. **파일명 `snake_case.md` 통일**: 크로스 플랫폼 및 스크립트 호환을 위해 공백·이모지 배제
5. **1파일 1주제 (Atomic Note)**: `# H1 -> ## H2 -> ### H3` 계층 유지

---

## 5. 새 프로젝트 추가 방법

1. `10_Projects/<new-project-name>/` 폴더 생성
2. 하위에 `00_MOC/`, `01_Core_Rules/`, `02_Specs_and_Skills/`, `04_ADR_and_Logs/` 구성
3. `90_Templates/active_context_template.md`를 복사해 `activeContext.md` 작성
4. 본 문서의 **2. 관리 중인 프로젝트** 목록에 링크 추가
