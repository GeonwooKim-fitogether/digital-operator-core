---
id: propose-skill-update
name: propose-skill-update
version: "0.1.0"
applies_to: [skill-refinement]
requires_approval: true
---

# propose-skill-update

Episode에서 재사용 가치가 있는 경험을 Skill 개선안으로 변환하는 스킬.

## 트리거 조건

- Learning Extractor가 Skill Candidate를 출력한 직후

## 핵심 원칙

**이 스킬은 Skill 파일을 직접 수정하지 않는다. 제안 파일만 생성한다.**

## 실행 단계

1. **가치 평가**: 새 Skill이 필요한지, 기존 Skill 수정인지 판단
2. **기존 Skill 확인**: 대상 Skill의 현재 내용 확인
3. **현재 문제 식별**: 기존 Skill에 무엇이 빠졌는가
4. **변경 수스 작성**: Diff 포함 제안 작성
5. **적용 범위 판단**: company (회사 전용) vs core (공통 승격 필요)
6. **회귀평가 계획**: 어떤 테스트로 검증할지 정의
7. **Proposal 파일 생성**: `skill-proposal.schema.json` 준수

## 출력

- `skill-proposal.schema.json` 준수 Skill Proposal
- 파일은 runtime/outputs/에 저장한다.

## 채택 기준

다음 조건을 모두 충족할 때만 Skill 개선안을 작성한다.

- 2개 이상의 Episode에서 반복된 패턴
- 확인 가능한 개선 효과가 있음
- 실행 가능한 회귀평가 계획이 있음
