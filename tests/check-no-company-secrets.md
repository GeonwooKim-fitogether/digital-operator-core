# Core 기밀 부재 체크리스트

Core 저장소에 회사 기밀이 들어가지 않았는지 확인하는 수동 체크리스트.
PR 병합 전 반드시 확인한다.

## 체크리스트

- [ ] 실제 회사명이 코드나 문서에 없다 (company-a는 구조 참조용 이름일 뿐)
- [ ] API Key, Token, Secret이 없다
- [ ] Supabase URL/Key가 없다
- [ ] Google OAuth Credential이 없다
- [ ] 실제 고객명, 재무 수치, 계약 내용이 없다
- [ ] Google Drive 원본 파일이 복사되어 있지 않다
- [ ] Project Episode 실제 데이터가 없다 (예제는 company-a에만)
- [ ] skill-proposal.schema.json의 requiresApproval이 const: true이다
- [ ] learning-policy.yaml에 auto_merge_without_review가 prohibited에 있다

## 자동 검사 제안 (후속)

향후 CI에서 secret 패턴을 검사하는 스크립트를 추가한다.
