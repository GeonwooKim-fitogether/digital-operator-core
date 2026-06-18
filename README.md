# digital-operator-core

조직 학습형 디지털 운영자의 공통 능력 저장소.

여러 Company Instance(company-a-operator, company-b-operator …)가 공유하는 실행 루프, Agent, Skill, Schema, 평가 Framework, 정책을 담는다.

## 핵심 원칙

1. Core는 회사에 종속되지 않는다 — 회사명, 기밀, 인증정보를 포함하지 않는다.
2. Skill 변경은 제안 → 평가 → 사람 승인 → 반영 순서를 따른다.
3. Company Instance는 Core를 Fork하지 않고 Connector Interface를 구현해 확장한다.
4. 출처 없는 사실은 조직 기억으로 승격하지 않는다.

## 학습 루프

```
Execute → Evaluate → Extract → Refine
```

각 단계의 상세 설명은 [docs/learning-loop.md](docs/learning-loop.md)를 참고한다.

## 구조

```
digital-operator-core/
├─ .claude-plugin/      # Claude Code Plugin 매니페스트
├─ agents/              # 공통 Agent 정의
├─ skills/              # 재사용 가능한 공통 Skill
├─ schemas/             # 공통 JSON Schema
├─ evals/               # 평가 Framework, Rubric, Fixture
├─ policies/            # 안전·승인·학습 정책
├─ docs/                # 설계 문서, ADR
└─ tests/               # 회귀평가 테스트
```

## Company Instance 연결

각 회사 저장소는 이 Core의 Connector Interface를 구현해 회사별 GBrain, Google Drive에 연결한다.

```
digital-operator-core      ← 공통 능력 (이 저장소)
  ↑ implements
company-a-operator         ← 회사 A 전용 환경
company-b-operator         ← 회사 B 전용 환경 (미래)
```

## Source of Truth 요약

| 시스템 | 역할 |
|---|---|
| GitHub (이 저장소) | Agent, Skill, Schema, 정책의 Source of Truth |
| Google Drive | 회사 원본 문서의 Source of Truth |
| GBrain + Supabase | 조직의 장기 기억과 판단 계층 |
| Claude Code | 사용자와의 단일 인터페이스이자 오케스트레이터 |

## 관련 문서

- [비전](docs/vision.md)
- [아키텍처](docs/architecture.md)
- [학습 루프](docs/learning-loop.md)
- [회사 격리 및 능력 승격](docs/company-isolation.md)
