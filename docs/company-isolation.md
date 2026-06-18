# 회사 격리 및 공통 능력 승격

## 회사별 독립 자원

각 회사는 다음을 독립적으로 갖는다.

- GitHub Company Instance 저장소
- Google Shared Drive
- GBrain 인스턴스
- Supabase 프로젝트
- 인증정보
- 정책 및 KPI
- 회사 전용 Skill
- 프로젝트 기억 (Episode)

회사 A의 원본 경험과 기밀을 회사 B로 직접 복사하지 않는다.

## Core의 역할

Core는 회사에 종속되지 않는다. 회사 기억과 Runtime 상태는 Company Instance에 남긴다.

```
digital-operator-core    ← Stateless 공통 능력
  ↑ implements
company-a-operator       ← 회사 A 기억 + 정책 + Connector
company-b-operator       ← 회사 B 기억 + 정책 + Connector (미래)
```

## 공통 능력 승격 절차

회사 A에서 검증된 방법을 Core에 반영하는 절차.

```
회사 A의 Episode
→ 일반화 가능한 패턴 추출
→ 회사명·고객명·수치·기밀 제거
→ 공통 Skill 개선안 생성
→ Core 회귀평가
→ 사람 승인
→ digital-operator-core 새 버전
→ 회사별로 선택적 업데이트
```

## 격리 경계

| 항목 | Core | Company Instance |
|---|---|---|
| 회사명 | ❌ | ✅ |
| 회사 기밀 | ❌ | ✅ |
| API Key / Secret | ❌ | ✅ (환경변수만) |
| GBrain 연결 설정 | Interface만 | 실제 구현 |
| Drive 연결 설정 | Interface만 | 실제 구현 |
| 공통 Agent | ✅ | 상속 |
| 공통 Skill | ✅ | 상속 + 확장 가능 |
| 회사 전용 Skill | ❌ | ✅ |
| Project Episode | ❌ | ✅ |
| 회사 KPI | ❌ | ✅ |

## 정보 흐름 규칙

- 회사 A의 기밀 → Core: ❌ 금지
- 회사 A의 기밀 → 회사 B: ❌ 금지
- 회사 A Episode (기밀 제거) → Core Skill Proposal: ✅ 허용 (승인 후)
- Core Skill → 회사 A: ✅ 허용 (선택적 업데이트)
