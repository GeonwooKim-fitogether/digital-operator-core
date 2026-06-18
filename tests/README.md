# 테스트

Core의 테스트는 다음을 검증한다.

1. **Schema 유효성**: 모든 JSON Schema가 valid JSON Schema인가
2. **회사 기밀 부재**: Core에 회사명/기밀이 들어있지 않은가
3. **Skill Proposal 불변성**: skill-proposal의 requiresApproval이 항상 true인가

## 실행

Phase 1에서는 Company Instance의 Mock Vertical Slice가 Schema를 실제로 사용해 검증한다.
Core 단독 테스트는 `check-no-company-secrets.md`의 체크리스트를 따른다.
