---
id: close-project
name: close-project
version: "0.1.0"
applies_to: [project-closure]
requires_approval: false
---

# close-project

프로젝트를 마감하고 Episode를 저장하는 스킬.

## 트리거 조건

- `evaluate-result`가 pass 또는 revise 후 수정 완료
- 사용자가 프로젝트 마감 요청

## 실행 단계

### 1단계: 최종 결과물 확인

- 사용자 확인
- Evaluation verdict 점검

### 2단계: `extract-learning` 호출

- Learning Extractor Agent 호출으로 Episode 생성

### 3단계: Episode 저장 승인 요청

- 사용자에게 Episode 내용 확인 요청
- 승인 시 GBrain에 저장 (gbrain.saveEpisode)

### 4단계: `propose-skill-update` 호출

- Skill Refiner Agent에게 Skill Candidate 전달
- Skill Proposal 생성 및 사용자 제안

### 5단계: Follow-up Tasks 전달

- 사용자에게 followUpTasks 목록 안내

## 출력

- GBrain에 저장된 Episode
- Skill Proposal (승인 대기)
- Follow-up Tasks 요약

## 주의

- Episode를 GBrain에 저장하기 전 반드시 사람 승인을 받는다.
- Skill Proposal은 제안만 하며, Skill 파일을 직접 수정하지 않는다.
