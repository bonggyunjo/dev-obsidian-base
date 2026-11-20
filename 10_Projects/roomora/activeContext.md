---
description: Roomora 프로젝트 현재 작업 상태 인수인계서 (50줄 이내).
layer: L0.5
related:
  - ./00_MOC/project_moc.md
---

# Active Context — Roomora

## 현재 단계
- **풀스택 1차 구축 완료 → 성능 검증(`k6` / `EXPLAIN ANALYZE`) 및 배포 준비 단계**

## 방금 완료한 일
- `roomora-server`: CORS 설정 + [RoomoraDataSeeder.java](file:///C:/Users/kjbg4/OneDrive/Desktop/roomora-server/src/main/java/com/roomora/server/global/config/RoomoraDataSeeder.java) (가이드 4종·레시피 4종·콘테스트 1회차 + `pgvector` 색인) 추가, **전체 28/28 테스트 통과**
- `roomora-web`: Nuxt 3 SSR + Pinia + Tailwind 전체 화면(`/`, `/search`, `/recipes`, `/contest`, `/mypage`, `/login`, `/users/[id]`) 구현, **`nuxi typecheck` + `nuxt build` 통과**

## 다음 할 일 (Next)
1. 로컬 풀스택 기동 확인 (`http://localhost:3000` ↔ `http://localhost:8080`)
2. **`k6` 부하 테스트 & `EXPLAIN ANALYZE` 수치화** (`test_strategy.md` 4장)
3. 배포 플랫폼 확정 후 CD 파이프라인 연결

## 미결정 / 주의사항
- `roomora-server` 로컬 실행 시 `5432` 포트 충돌 주의 (`ERR-001`)
