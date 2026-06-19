# CHANGELOG

## [0.2.0] — Phase 2 준비: GBrain HTTP MCP 연결 설계

### Added
- `docs/adr/ADR-002-gbrain-integration.md`: GBrain HTTP MCP 직접 연결 결정 기록
- `docs/phase2-roadmap.md`: Phase 2 단계별 체크리스트 (Ollama → GBrain → Supabase)

## [0.1.0] — Phase 1: Architecture + Skeleton + Mock Vertical Slice

### Added
- Core 저장소 초기 구조 설정
- 공통 Agent 초안: executor, evaluator, learning-extractor, skill-refiner
- 공통 Skill 초안: run-project, close-project, evaluate-result, extract-learning, propose-skill-update
- 공통 JSON Schema 6종
- 기본 평가 Framework 및 Default Rubric
- 안전·승인·학습 정책
- 설계 문서: vision, architecture, learning-loop, company-isolation
- Claude Code Plugin 매니페스트
- 기본 회귀평가 테스트 구조
