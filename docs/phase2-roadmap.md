# Phase 2 로드맵: 실제 GBrain/Supabase 연결

> **상태**: 진행 중  
> **시작일**: 2026-06-18  
> **Phase 1 완료**: Mock Vertical Slice 전체 루프 증명 완료

---

## 목표

Mock 어댑터를 실제 GBrain HTTP MCP + Supabase로 교체해
실제 조직 기억이 쌓이는 시스템을 만든다.

---

## 단계 1: 임베딩 제공자 결정 및 Ollama 설치

이 단계는 company-a-operator에서 진행한다.
- [ ] `company-a-operator/docs/embedding-provider-decision.md` 검토
- [ ] Ollama 설치: `company-a-operator/docs/ollama-setup.md` 참고
- [ ] `ollama pull nomic-embed-text` 완료
- [ ] Ollama 동작 확인: `curl http://localhost:11434`

## 단계 2: GBrain 버전 고정

- [ ] GBrain 공식 저장소에서 안정 커밋 SHA 확인
- [ ] `bun install -g github:garrytan/gbrain@<SHA>` 로 버전 고정
- [ ] `gbrain --version` 확인

## 단계 3: Supabase 프로젝트 설정

- [ ] Supabase 프로젝트 생성
- [ ] pgvector 확장 활성화: `create extension if not exists vector;`
- [ ] GBrain 마이그레이션: `gbrain migrate --to supabase`

## 단계 4: HTTP MCP 서버 기동

- [ ] `gbrain serve --http --port 3131` 기동
- [ ] 실제 tool 이름 확인: `gbrain tools`
- [ ] `company-a-operator/adapters/gbrain/gbrain-mcp-adapter.ts` TODO 상수 업데이트

## 단계 5: 통합 검증

- [ ] company-a-operator에서 `USE_MOCK_ADAPTERS=false npm test` 통과
- [ ] GBrain에 저장된 Episode를 `gbrain search`로 조회 성공
- [ ] Cross-company 접근 거부 확인

---

## Core 역할 분리 원칙

- Core는 어댑터 인터페이스 정의 + 공통 로직만 담당
- 실제 GBrain/Supabase/Drive 연결은 회사별 operator에서
- Core에는 회사 A의 실제 연결 정보가 들어오지 않는다
