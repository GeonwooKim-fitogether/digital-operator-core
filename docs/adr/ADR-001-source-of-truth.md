# ADR-001: Source of Truth 분리 결정

**날짜**: 2025-06  
**상태**: 승인됨

## 배경

조직 학습형 디지털 운영자 시스템에서 데이터와 능력을 어디에 저장할지 결정이 필요했다. 단일 시스템에 모두 넣을 경우 유연성이 떨어지고, 회사별 격리가 어렵다.

## 결정

역할에 따라 Source of Truth를 명확히 분리한다.

| 시스템 | Source of Truth 역할 |
|---|---|
| Google Drive | 회사 원본 문서 |
| GBrain + Supabase | 조직의 장기 기억과 판단 |
| GitHub | Agent, Skill, Schema, 정책 |
| Claude Code | 오케스트레이터 (저장소 아님) |

## 근거

- **Google Drive**: 회사 구성원이 이미 사용하는 공식 문서 저장소. 중복 복제는 동기화 문제를 일으킨다.
- **GBrain + Supabase**: 원본이 아닌 학습된 지식 저장. 여러 사용자가 같은 기억을 공유해야 한다.
- **GitHub**: 버전 관리, 리뷰, 승인 프로세스가 이미 내장되어 있어 Skill 진화 추적에 적합하다.

## 결과

- Google Drive를 Supabase로 복제하지 않는다.
- GBrain은 Drive 원본이 아니라 Drive에서 추출된 판단과 기억을 저장한다.
- Skill과 정책 변경은 GitHub PR을 통해 이루어진다.

## 재검토 조건

- GBrain이 Drive와의 직접 연동 기능을 제공하게 되면 재검토한다.
- 오프라인 환경 요구가 생기면 재검토한다.
