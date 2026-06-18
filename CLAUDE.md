# CLAUDE.md — digital-operator-core

이 파일은 Claude Code가 이 저장소에서 작업할 때 따라야 하는 규칙이다.

## 이 저장소의 역할

이 저장소는 **공통 능력만** 담는다. 회사 A, B, C에 배치되는 Company Instance가 공유하는 Agent, Skill, Schema, 평가 기준, 정책을 제공한다.

## 절대 하지 않을 것

- 회사명, 회사 기밀, 특정 회사의 KPI, 고객 정보를 이 저장소에 포함하지 않는다.
- Supabase Secret, Google OAuth, API Key 등 인증정보를 커밋하지 않는다.
- 실제 GBrain 명령이나 Schema를 공식 문서 확인 없이 추측해 구현하지 않는다.
- `digital-operator-workspace/` 상위 폴더에 파일을 생성하지 않는다.
- 운영 Skill을 사람 승인 없이 직접 수정하지 않는다.
- Agent가 `skills/` 디렉터리를 직접 덮어쓰지 않는다.

## Skill 변경 절차

1. `schemas/skill-proposal.schema.json`에 맞는 Skill Proposal을 작성한다.
2. 관련 Episode와 근거를 명시한다.
3. 회귀평가 계획을 포함한다.
4. 사람이 검토하고 승인하면 반영한다.

## 사람 승인이 필요한 작업

- `skills/` 내 파일 수정
- `policies/` 내 파일 수정
- `agents/` 내 핵심 지시 변경
- `schemas/` 변경 (하위 호환 깨지는 경우)
- Core를 Company Instance에 배포

## Source of Truth

| 시스템 | 역할 |
|---|---|
| GitHub (이 저장소) | Agent, Skill, Schema, 정책의 Source of Truth |
| Google Drive | 회사 원본 문서의 Source of Truth |
| GBrain + Supabase | 조직의 장기 기억과 판단 계층 |
| Claude Code | 사용자와의 단일 인터페이스이자 오케스트레이터 |

## 커밋 규칙

- Core 변경과 Company Instance 변경은 각 저장소에 분리 커밋한다.
- 커밋 메시지는 변경된 능력이나 정책을 구체적으로 설명한다.
- `feat:` 새 Skill/Agent, `fix:` 버그 수정, `docs:` 문서, `policy:` 정책 변경, `schema:` Schema 변경
