# CLAUDE.md — digital-operator-core

이 파일은 Claude Code가 이 저장소를 열 때 가장 먼저 읽는 맥락이다.
규칙 목록이 아니라 **이 시스템을 왜 만들었는지, 어떤 생각으로 설계했는지**부터 설명한다.

---

## 왜 이 시스템을 만들었는가

### 해결하려는 문제

회사에서 반복되는 현상이 있다.

- 사람이 퇴사하면 그 사람이 알고 있던 맥락, 판단 근거, 시행착오가 함께 사라진다
- 같은 종류의 프로젝트를 할 때마다 처음부터 다시 시작한다
- "우리가 왜 그때 그 결정을 했지?"를 아무도 모른다
- 좋은 방법을 발견해도 다음에 그걸 쓰지 않는다

단순히 "질문에 답하는 AI"나 "문서를 유리하게 업로드하는 도구"로는 해결되지 않는다.
**프로젝트를 수행할수록 조직이 똑똑해지는 구조**가 필요하다.

### 핵심 아이디어

> 실행할수록 학습하는 디지털 직원을 만든다.

이 시스템은 AI가 프로젝트를 처리할 때마다:
1. 과거 경험을 찾아서 활용하고
2. 결과를 스스로 평가하고
3. 배운 것을 조직 기억에 남기고
4. 반복되는 패턴을 업무 능력(Skill)으로 추상화한다

단순 자동화가 아니라 **조직 학습**이다.

---

## 설계 철학

### 1. 실행이 아니라 학습이 목적이다

프로젝트 하나를 잘 끝내는 것보다, **그 프로젝트에서 무엇을 배웠는가**가 더 중요하다.
평가 단계가 독립적인 이유도 이 때문이다 — 자기 결과를 스스로 콡슬하는 루프는 학습이 아니다.

### 2. 사람이 통제의 마지막 문이다

AI가 Skill을 스스로 바꾸거나, 회사 기밀을 외부로 보내거나, 계약을 확정하는 일은 없다.
이 시스템은 **제안까지만 자동화**하고, 반드시 사람이 검토하고 승인해야 반영된다.
`requiresApproval: true`가 스키마에 상수로 박혀 있는 이유다.

### 3. Source of Truth는 역할별로 분리한다

한 시스템이 모든 것을 담으면 경계가 무너진다. 역할을 명확히 나눐다:

| 시스템 | 역할 | 왜 따로 |
|---|---|---|
| **Google Drive** | 사람이 만든 원본 문서 | AI가 덮어쓰면 안 되는 진짜 원본 |
| **GBrain + Supabase** | AI가 학습한 조직 기억 | Drive를 대체하지 않음 — 소화된 지식만 담음 |
| **GitHub (이 저장소)** | AI의 업무 능력과 정책 | Skill, Agent, Schema, 승인 기준 |
| **Claude Code** | 사용자와의 단일 인터페이스 | 사용자는 Claude Code 하나만 상대함 |

Drive에 있는 파일을 GBrain에 통째로 복사하지 않는다.
GBrain에는 Drive를 읽고 소화한 결과(판단, 결론, 패턴)만 남긴다.

### 4. Core는 철저히 무상태(Stateless)다

Core는 회사 A가 무엇을 했는지, 어떤 기밀이 있는지 알지 못한다.
Core가 아는 것은 "어떻게 일하는가" — 학습 루프, 평가 기준, Skill 구조뿐이다.

이 설계 덕분에 같은 Core를 회사 A, B, C에 독립 배치할 수 있다.
각 회사의 기억은 절대 섭이지 않는다.

### 5. GBrain은 추측하지 않는다

GBrain (~v0.30)은 자주 인터페이스가 바뀐다.
공식 문서 확인 없이 tool 이름이나 파라미터를 추측해서 구현하면 조용히 망가진다.
항상 실제 `gbrain tools` 출력으로 확인한 뒤 구현한다.

---

## 학습 루프: Execute → Evaluate → Extract → Refine

이 루프가 시스템의 심장이다. 각 단계가 왜 분리되어 있는지 이해해야 한다:

```
Execute  → 과거 기억 + Drive 원본을 참고해 실제 결과물을 만든다
Evaluate → Execute와 독립된 관점으로 결과를 냉정하게 평가한다 (자화자찬 금지)
Extract  → "무엇이 왜 됔고 안 되는가"를 다음에 쓸 수 있는 Episode로 기록한다
Refine   → 반복 패턴이 확인되면 Skill 개선 제안을 생성한다 (직접 적용은 금지)
```

**Evaluate가 독립적이어야 하는 이유**: Executor가 자기 결과를 평가하면 무의식적으로 합리화한다.
평가자는 반드시 다른 관점에서 접근해야 하고, 실패를 명확히 기록해야 한다.

**Refine에서 Skill을 직접 수정하지 않는 이유**: 검증 안 된 패턴이 운영 Skill에 반영되면
모든 다음 프로젝트에 영향을 준다. 제안과 승인을 분리해야 안전하게 진화할 수 있다.

---

## 이 저장소의 역할 (Core)

`digital-operator-core`는 회사 무관한 **공통 업무 능력**만 담는다.

- `agents/` — 각 단계를 담당하는 Agent 지시서 (executor, evaluator, learning-extractor, skill-refiner)
- `skills/` — 검증된 업무 절차 (run-project, evaluate-result, extract-learning 등)
- `schemas/` — 데이터 계약 6종 (project-brief, execution-plan, source-reference, evaluation, project-episode, skill-proposal)
- `evals/` — 평가 기준 (default rubric, scoring formula, fixtures)
- `policies/` — 자동화 가능 작업 vs 사람 승인 필요 작업 기준
- `docs/` — 설계 결정 기록 (ADR 포함)

이 저장소에 **절대 들어오면 안 되는 것**: 회사명, 회사 기밀, 특정 회사 KPI, API Key, .env, runtime 산출물

---

## 현재 상태 (2026-06 기준)

### Phase 1 완료 ✅
- Mock 어댑터로 **Execute→Evaluate→Extract→Refine 전체 루프 동작 증명**
- `company-a-operator`에서 `npm test`로 검증 가능
- 시드 Episode EP-001 존재 (시장 분석 프로젝트 학습 기록)

### Phase 1.5 완료 ✅
- GBrain 공식 인터페이스 조사 완료 (`garrytan/gbrain` 기준)
- ADR-002: 커스텀 REST 대신 **GBrain HTTP MCP 직접 연결** 결정
- GBrain MCP 어댑터 스켈레톤 작성 (tool 이름 확인 후 채워야 하는 TODO 포함)
- 임베딩 제공자 결정: **Ollama 로컬 우선** (외부 전송 없음, 사람 승인 불필요)

### Phase 2 진행 중 🔄
- 실제 `gbrain serve --http` 연결 준비
- Supabase 마이그레이션 예정
- `company-a-operator/docs/phase2-roadmap.md` 참고

---

## 작업 규칙

### 절대 하지 않는 것
- 회사 기밀, 특정 회사 KPI, 인증정보를 이 저장소에 포함하지 않는다
- `skills/` 또는 `agents/`를 사람 승인 없이 직접 수정하지 않는다
- GBrain tool 이름/파라미터를 추측으로 구현하지 않는다 (`gbrain tools`로 확인 먼저)
- `digital-operator-workspace/` 상위 폴더에 파일을 만들지 않는다

### Skill 변경 절차
1. `schemas/skill-proposal.schema.json` 준수 Proposal 작성
2. 근거 Episode(≥2개), 예상 효과, 회귀평가 계획 포함
3. `requiresApproval: true` 필수
4. 사람 검토 후 승인된 것만 `skills/`에 반영

### 커밋 컨벤션
```
feat:    새 Skill/Agent 추가
fix:     버그 수정
docs:    문서 변경
policy:  정책 변경
schema:  Schema 변경
refactor: 구조 개선 (동작 변경 없음)
```

Core 변경과 Company Instance 변경은 각 저장소에 분리 커밋한다.
