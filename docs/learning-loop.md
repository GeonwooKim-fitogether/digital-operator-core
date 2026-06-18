# 핵심 학습 루프

시스템의 핵심은 네 단계의 순환이다.

```
Execute → Evaluate → Extract → Refine
```

---

## 1. Execute

**입력**
- Project Brief (목표, 완료조건, 제약)
- 관련 과거 Episode (GBrain)
- 관련 Drive 원본 (Google Drive)

**수행**
1. 목표 명확화
2. 작업 분해
3. 위험 및 의존성 확인
4. 실행계획 수립
5. 실제 산출물 작성
6. 출처 기록

**출력**
- `execution-plan.schema.json` 준수 Execution Plan
- 작업 결과물
- Source Manifest
- 미해결 사항
- 판단이 필요한 사항

---

## 2. Evaluate

Executor와 **분리된 관점**으로 결과를 검토한다. Evaluator는 Executor의 결과를 단순 칭찬하지 않는다.

**평가 기준**
- 목표 달성도
- 완료조건 충족 여부
- 사실 정확성
- 출처 추적 가능성
- 누락 및 갭
- 리스크
- 회사 정책 준수
- 재사용 가능성

**출력**
- `evaluation.schema.json` 준수 Evaluation Report
- 점수 및 Pass / Revise / Fail 판정
- 근거 및 수정 요구사항
- 불확실성 목록

---

## 3. Extract

프로젝트에서 다음 프로젝트로 가져갈 경험을 추출한다.

**추출 대상**
- 무엇이 잘 작동했는가
- 무엇이 실패했는가, 근본 원인은 무엇인가
- 처음의 가정 중 무엇이 틀렸는가
- 예상보다 비용이 컸던 단계
- 재사용 가능한 절차
- 회사 전용 지식인가, 일반화 가능한 능력인가
- 추가로 확인해야 할 Knowledge Gap

**출력**
- `project-episode.schema.json` 준수 Project Episode
- Decision Record
- Playbook Candidate
- Skill Candidate
- Follow-up Tasks

---

## 4. Refine

재사용 가치가 충분한 경험만 Skill 개선안으로 바꾼다. **모든 교훈을 Skill에 넣지 않는다.**

**Skill 개선안 필수 포함 항목**
- 대상 Skill
- 현재 문제
- 제안 변경
- 변경 근거 (관련 Episode)
- 예상 이점 및 부작용
- 회귀평가 계획
- 적용 범위: company 또는 core
- 승인 필요 여부

**원칙**
- Agent가 운영 Skill을 직접 수정하지 않는다.
- Skill Proposal은 제안까지만 자동화한다.
- 사람이 검토하고 승인해야 반영된다.

---

## 진화 절차

```
프로젝트 실행
→ 결과 평가
→ 성공·실패 원인 추출
→ 개선안 생성
→ 기존 Skill과 비교
→ 과거 사례로 회귀 평가
→ 사람이 검토
→ 승인된 개선안만 반영
```
