---
id: extract-learning
name: extract-learning
version: "0.1.0"
applies_to: [learning-extraction]
requires_approval: false
---

# extract-learning

프로젝트 경험에서 다음 프로젝트로 가져갈 교훈을 추출하는 스킬.

## 트리거 조건

- Evaluation이 완료된 직후
- `close-project` 단계 중

## �심

**종합 평가가 아니라 실제 유용한 교훈을 줌산하는 것이 이 스킬의 햸심이다.**

## 실행 단계

1. Execution Plan 및 Evaluation 검토
2. 성공 요인과 실패 요인 독립적으로 분리
3. 각 실패의 근본 원인 식별 (단순 증상이 아니라 원인)
4. 잘못된 가정 확인
5. 재사용 가능한 패턴 추출
6. companySpecific 여부 판단
7. Knowledge Gap 식별
8. `project-episode.schema.json` 준수 Episode 작성

## 출력

- `project-episode.schema.json` 준수 Episode
- Skill Candidate 목록 (다음 단계 Refiner에게 전달)
- Follow-up Tasks

## 주의

- Episode에 evaluationId와 sourceReferenceIds를 반드시 포함한다.
- Episode은 Evaluation 업시 연결되어야 한다 — 평가 없는 Episode는 안 된다.
