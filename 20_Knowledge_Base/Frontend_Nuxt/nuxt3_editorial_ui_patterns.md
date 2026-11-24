---
description: Nuxt 3 SSR + Tailwind 에디토리얼 UI 및 4대 엣지 상태(Empty/Skeleton/Error/Overflow) 체크리스트.
layer: L2
related:
  - ../knowledge_moc.md
---

# Nuxt 3 Editorial UI & State Checklist

## 1. 에디토리얼 디자인 & 상태 설계
- 과한 그라데이션·이모지 남발·Card-in-Card 중첩을 피하고, 웜 크림 배경(`#FAF9F6`) + 딥 차콜 CTA(`zinc-900`) + 테라코타 포인트(`#8C583A`) 위계를 유지한다.
- 모든 화면은 **Empty / Skeleton / Error / Overflow** 4대 상태와 **Default / Hover / Active / Disabled / Focus** 버튼 상태를 갖춘다.

## 2. SSR & 카피라이팅 규칙
- `window`/`document` 접근은 `import.meta.client` 가드 내부로 제한하고, 브라우저 기본 `alert()`/`confirm()` 대신 전역 Toast/Confirm 모달 스토어를 사용한다.
- UI 문구에는 `RAG`, `Citation`, `1-Click`, `404` 같은 개발자/AI 내부 용어를 노출하지 않고 사용자 언어("맞춤 제안", "참고 공간")로 번역한다.
