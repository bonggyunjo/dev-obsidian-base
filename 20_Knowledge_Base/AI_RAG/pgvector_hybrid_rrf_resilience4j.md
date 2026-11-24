---
description: PostgreSQL pgvector HNSW + pg_trgm 하이브리드 RRF 검색 및 Resilience4j Fallback 재사용 노트.
layer: L2
related:
  - ../knowledge_moc.md
---

# pgvector Hybrid RRF & Resilience4j Fallback

## 1. 단일 DB 하이브리드 검색 구조
- 별도 Vector DB 없이 PostgreSQL 16 단일 DB에서 정형 조건 Pre-filtering 후 `pgvector` HNSW(`vector_cosine_ops`)와 `pg_trgm` 키워드 순위를 RRF(Reciprocal Rank Fusion)로 결합한다.
- 청크 테이블에는 `(source_type, source_id, chunk_index)` Unique 제약을 두어 재색인 시 멱등 Upsert가 가능하다.

## 2. 장애 격리 및 비용 절감 패턴
- **LLM 장애 우회**: `@CircuitBreaker` Fallback으로 외부 LLM 장애 시 DB 조건·키워드 기반 `200 OK` (`fallback: true`)를 반환한다.
- **임베딩 폴백**: API 키 미설정/할당량 초과 시 결정적 1536차원 로컬 벡터로 폴백해 시딩·색인 중단을 방지한다.
- **Redis 캐싱**: 동일 질의·조건 조합은 Redis에 캐싱하여 반복 응답을 P95 10ms 미만으로 단축한다.
