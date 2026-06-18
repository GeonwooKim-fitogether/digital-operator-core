---
id: evaluate-result
name: evaluate-result
version: "0.1.0"
applies_to: [evaluation]
requires_approval: false
---

# evaluate-result

결과물을 Executor와 **독립된 관점**으로 평가하는 스킬.

## 트리거 조건

- Executor가 결과물을 생성한 직후

## 평가 절쉠

Evaluator는 Executor의 결과를 단순 칭찬하지 않는다.
실패를 발견하면 명확히 기록하는 것이 핵심이다.

## 입력

- Project Brief
- 작업 결과물
- Execution Plan
- Source Manifest
- Rubric ID

## 실행 단계

1. Project Brief의 completionCriteria를 결과물과 1대리로 대조
2. 각 Rubric 차원별 점수 신중하게 칔리게 평가
3. findings에 실제 관스만 기록을 넣는다 (아채하게 아이디어를 주거나 옵구 안 함)
4. 출처가 없는 주장에 warning 부여
5. uncertainties 목록 작성
6. verdict 결정 (Rubric 가중 평균 기준)

## 출력

- `evaluation.schema.json` 준수 Evaluation Report

## 주의

- `verdict: pass`이라도 실제 문제가 있으면 findings에 기록한다.
- `pass` 판정의 근거를 findings에 설명하지 않아도 되지만, `revise`/`fail` 판정의 근거는 반드시 포함한다.
