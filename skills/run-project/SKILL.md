---
id: run-project
name: run-project
version: "0.1.0"
applies_to: [project-execution]
requires_approval: false
---

# run-project

프로젝트 실행 흐름 전체를 실행하는 스킬.

## 트리거 조건

사용자가 `run-project`, 프로젝트 실행, 또는 Project Brief를 제공할 때.

## 사전 요구사항

- `project-brief.schema.json` 준수 Project Brief
- GBrain Adapter (실제 또는 Mock)
- Google Drive Adapter (실제 또는 Mock)

## 실행 단계

### 1단계: 목표 명확화

- Project Brief의 objective, completionCriteria, constraints 확인
- 불명확한 사항은 사용자에게 확인 요청

### 2단계: 과거 경험 조회

- GBrain에서 관련 Episode 조회 (Executor agent 코드의 gbrain.queryEpisodes 호출)
- 재사용 가능한 패턴과 피해야 할 실패 패턴 확인

### 3단계: 데이터 접근 가능성 사전 확인

- 필요한 데이터 소스 목록 작성
- 각 소스의 접근 가능 여부 확인 (Drive 권한, 외부 구독 등)
- 접근 불가 시 Knowledge Gap으로 등록, 대안 탐색

### 4단계: Google Drive 원본 조회

- 승인된 폴더에서 관련 문서 검색
- Drive 파일 ID, URL, 수정시각을 Source Reference에 기록

### 5단계: 실행계획 수립

- Task 분해 및 의존성 정의
- riskFlags 식별
- pendingDecisions 식별
- `execution-plan.schema.json` 준수 Plan 생성

### 6단계: 실제 산출물 작성

- Source Reference를 포함한 결과물 작성
- 출처 없는 주장은 `[출처 불명]`으로 명시
- Source Manifest (sourceReferenceIds) 완성

## 출력

- Execution Plan (runtime/outputs/)
- 결과물 (runtime/outputs/)
- Source Manifest
- pendingDecisions 목록

## 주의

- 결과물을 스스로 Evaluate하지 않는다. `evaluate-result` Skill을 별도 호출한다.
- pendingDecisions가 3개 초과 시 작업을 시작하지 않고 사용자에게 에스켈레이션한다.
