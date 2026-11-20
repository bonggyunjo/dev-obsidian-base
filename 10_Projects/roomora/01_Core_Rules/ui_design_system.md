---
description: Roomora 프론트엔드(roomora-web) 프로덕트급 UI/UX 디자인 시스템, 4대 상태, 마이크로 인터랙션, 카피라이팅 및 비즈니스 지표 연결 표준.
layer: L1
related:
  - ./system_architecture.md
  - ../04_ADR_and_Logs/decisions.md
---

# UI/UX Design System & Product Standards — Roomora

## 1. UX 설계 철학 (감정 · 여정 · 비즈니스 지표)
1. **버튼의 맥락 정의 (Why / Before / After)**: 모든 주요 CTA(검색, 체크리스트 담기, 투표, 팔로우)는 *"사용자가 왜 누르는가, 누르기 직전 망설임은 무엇인가, 누른 직후 어떤 안도감/보상을 얻는가"*를 기준으로 문구와 피드백을 설계한다.
2. **이탈 지점 선제 방어**: 첫 화면(온보딩) → 조건 탐색 → 체크리스트 저장 → 재방문(체크 실행)까지 사용자가 *"여기서 뭘 누르지?"* 하고 머뭇거릴 틈이 없도록 다음 행동을 1클릭 동선으로 연결한다.
3. **비즈니스 지표(CVR · 리텐션) 연결**:
   - **첫 화면 이탈률 감소**: 딱딱한 빈 검색창 대신 예산·주거 칩을 둬 클릭 한 번으로 즉시 탐색 진입
   - **전환율(CVR) 극대화**: 검색 결과에서 "체크리스트로 담기" 동선을 최상단·명확한 버튼으로 배치해 열람 → 저장 전환 유도
   - **리텐션(재방문율) 강화**: 저장한 체크리스트 진행률(`%`)과 주간 콘테스트 랭킹 변동으로 재방문 동기 부여

---

## 2. 정밀 디자인 토큰 (4px/8px 그리드 · 타이포 · 시맨틱 컬러)
- **4px / 8px 그리드 시스템**: 모든 패딩·마진·간격은 4px 배수(`gap-2`, `gap-4`, `gap-6`, `gap-8`, `p-4`, `p-6`, `p-8`)로 통일해 시각적 리듬을 맞춘다.
- **타이포그래피 위계 (Hierarchy)** (`Pretendard` + 시스템 산세리프):
  - `H1 (Page Title)`: `text-2xl md:text-4xl font-bold tracking-tight`
  - `H2 (Section)`: `text-lg md:text-xl font-bold tracking-tight`
  - `Body`: `text-sm md:text-base leading-relaxed`
  - `Caption / Meta`: `text-xs text-zinc-500`
- **시맨틱 컬러 시스템**:
  - **Surface / Text**: 배경 `#FAF9F6` (웜 아이보리), 카드 `#FFFFFF`, 기본 텍스트 `#18181B` (차콜), 보조 `#71717A`, 보더 `#E4E4E7`
  - **Primary**: `#18181B` (핵심 CTA) / **Accent**: `#8C583A` (우드·테라코타 포인트)
  - **Success**: `#15803D` (저장 완료·무타공) / **Warning**: `#B45309` (주의 경고) / **Danger**: `#DC2626` (삭제·에러)

---

## 3. 컴포넌트 5대 상태 & 4대 화면 상태 표준

### 버튼 5대 상태 필수 정의
모든 버튼은 **Default / Hover / Pressed(`active:scale-[0.98]`) / Disabled(`disabled:opacity-40 disabled:cursor-not-allowed`) / Focus(`focus-visible:ring-2 focus-visible:ring-zinc-900 focus-visible:ring-offset-2`)** 상태를 모두 갖춰야 한다.

### 4대 화면 상태 필수 설계
1. **Empty State**: 데이터가 0건일 때 빈 화면 방치 금지 → 아이콘/안내 문구 + 다음 행동 CTA 버튼 제공
2. **Loading State (Skeleton UI)**: API 대기 중 깜빡임이나 빈 박스 대신 `animate-pulse` 스켈레톤 카드 표시
3. **Error State**: 네트워크/서버 오류 시 사용자 친화적 안내 + **`[다시 시도]` 버튼** 제공
4. **Overflow 방어**: 긴 제목·품목명·메모에 `line-clamp-1`, `line-clamp-2`, `break-words`, `min-w-0` 적용

---

## 4. 입력 검증 · 예외 처리 · 마이크로 인터랙션
- **브라우저 기본 `alert()` / `confirm()` 사용 금지**
- **인라인 필드 검증**: 이메일 형식, 비밀번호 8자 미만, 예산 음수 등은 입력창 하단에 즉시 에러 메시지 표시
- **즉각적 피드백 (Toast & Confirm Modal)**:
  - 저장·좋아요·북마크·체크 완료 시 우측 하단 **Toast 알림**으로 즉각 반응
  - 게시글 삭제·체크리스트 삭제·회원 탈퇴 등 파괴적 작업 전에는 반드시 **확인 모달(Confirm Modal)** 표시

---

## 5. UX 카피라이팅 규칙 (AI/개발자 용어 금지)
화면 텍스트에는 기술 용어(`RAG`, `Citation`, `1-Click`, `Fallback`)를 절대 노출하지 않고 일상 한국어만 사용한다.

| 금지 표현 (AI/개발자 투) | 권장 표현 (실사용 서비스 언어) |
| :--- | :--- |
| AI RAG 통합 검색 | **내 조건으로 공간 찾기** |
| 맞춤 인테리어 처방 | **이 조건에 어울리는 공간 제안** |
| 참고한 근거 데이터 (Citations) | **비슷한 조건의 실제 방 사례** |
| 1-Click 내 체크리스트 저장 | **내 체크리스트로 담기** |
| Fallback 검색 결과 | **조건과 일치하는 레시피를 모았어요** |
