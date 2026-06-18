# 아키텍처

## 저장소 구조

```
digital-operator-core         ← 공통 능력 (이 저장소)
company-a-operator            ← 회사 A 전용 인스턴스
company-b-operator            ← 회사 B 전용 인스턴스 (미래)
```

두 저장소의 Git 이력은 독립적으로 유지한다.
상위 폴더(`digital-operator-workspace/`)에는 파일을 생성하지 않는다.

## 시스템 구성 요소

### Claude Code (오케스트레이터)

사용자가 상대하는 유일한 인터페이스. 목표를 받은 뒤 아래 흐름을 관리한다.

```
목표 이해
→ 과거 기억 조회 (GBrain)
→ 원본 자료 확인 (Google Drive)
→ 실행계획 수립
→ 작업 수행
→ 결과 평가
→ 경험 추출
→ 기억 저장
→ Skill 개선안 제안
```

### Google Drive (원본 Source of Truth)

- 보고서, 회의록, 설계자료, 스프레드시트, 발표자료 등 원본 문서
- Drive 전체를 GitHub나 Supabase에 복제하지 않는다.
- MVP에서는 Read-only
- 원본 파일 ID, URL, 수정시각 등 참조 정보를 보존

### GBrain + Supabase (조직 장기 기억)

- 원본 파일 전체가 아니라 학습된 지식을 저장
- 저장 대상: 프로젝트 목표/결과, 의사결정 근거, 성공/실패 사례, 재사용 가능 해결법
- Google Drive를 대체하지 않는다
- 여러 컴퓨터와 사용자가 동일한 GBrain을 공유
- 회사별 독립 환경

### GitHub (능력·정책 Source of Truth)

- Agent, Skill, 실행 루프, 평가 기준, JSON Schema, 승인 정책
- 저장하지 않는 것: Secret, 인증정보, Drive 원본, 민감한 Runtime 결과

## Connector Interface 패턴

Core는 추상 Interface만 정의한다. 실제 구현은 Company Instance가 제공한다.

```typescript
// Core가 정의하는 Interface
interface GBrainAdapter { ... }
interface GoogleDriveAdapter { ... }

// Company Instance가 구현
class CompanyAGBrainAdapter implements GBrainAdapter { ... }
class CompanyAGoogleDriveAdapter implements GoogleDriveAdapter { ... }

// Phase 1 Mock
class MockGBrainAdapter implements GBrainAdapter { ... }
class MockGoogleDriveAdapter implements GoogleDriveAdapter { ... }
```

## 데이터 흐름

```
사용자 → Claude Code
                ↓
        [Project Brief]
                ↓
     GBrain: 과거 Episode 조회
     Drive:  관련 원본 조회
                ↓
        [Execution Plan]
                ↓
          작업 수행
                ↓
        [결과물 + Source Manifest]
                ↓
     독립 Evaluator가 평가
                ↓
        [Evaluation Report]
                ↓
     Learning Extractor 실행
                ↓
        [Project Episode]
                ↓
     Skill Refiner 실행
                ↓
        [Skill Proposal] → 사람 검토 → (승인 시) Skill 반영
```

## 보안 경계

- 회사 A의 GBrain ↔ 회사 B의 GBrain: 격리
- Core: 회사 기밀 없음, Stateless
- Secret은 환경변수로 주입, 절대 커밋하지 않음
- 권한 통제: 향후 Google Workspace 권한 + Supabase RLS 정책으로 강제
