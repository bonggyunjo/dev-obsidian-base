---
description: aesthetics-of-space-v2 전체 시스템 아키텍처 및 기술 스택 기준서.
layer: L1
related:
  - ../00_MOC/project_moc.md
  - ../04_ADR_and_Logs/decisions.md
---

# System Architecture — aesthetics-of-space-v2

## 1. 프로젝트 개요
- **주제**: 조건 기반 인테리어 레시피 · 주간 콘테스트 · AI RAG 통합 검색 플랫폼
- **원본 레거시**: `The-Aesthetics-Of-Space-Project` (학부 캡스톤)

## 2. 기술 스택 (To-Be)
- **Backend**: Java 17+, Spring Boot 3.x, Spring Security + JWT + OAuth2 (Kakao, Google), Spring Data JPA + QueryDSL
- **Database & Vector**: PostgreSQL 16 + `pgvector` (단일 DB 하이브리드 검색)
- **Cache / Ranking / Rate Limit**: Redis (Refresh Token, ZSET 실시간 랭킹, RAG 질의 캐싱, Rate Limit)
- **AI / Resilience**: Spring AI, `@TransactionalEventListener(AFTER_COMMIT)` 비동기 임베딩, Resilience4j (Circuit Breaker + Fallback)
- **Storage**: AWS S3 또는 Cloudflare R2 (Presigned URL)
- **Frontend**: Nuxt 3 (SSR) + Pinia + Tailwind CSS
- **Infra & Test**: Docker Compose, GitHub Actions, k6
