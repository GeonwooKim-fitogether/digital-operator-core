# 평가 Framework

Core의 평가 Framework는 결과물을 Rubric 기준으로 채점하고 verdict를 산출하는 순수 로직이다.

## 구성

```
evals/
├─ framework/        # 채점 로직 (언어 무관 설명)
├─ rubrics/          # 평가 기준 (YAML)
└─ fixtures/         # 테스트 고정 입력/출력
```

## 채점 방식

1. 각 Rubric 차원에 1-5 점수를 부여한다.
2. 차원별 가중치를 곱해 0-100 overallScore를 계산한다.
3. verdict 기준과 비교해 pass/revise/fail을 결정한다.
4. 특정 차원의 최소 점수 조건(예: policy-compliance)을 강제한다.

## 계산식

```
overallScore = Σ (dimensionScore / 5 * weight) * 100
```

## Company Instance 연결

Company Instance는 `default-rubric.yaml`을 상속해 회사별 Rubric을 만든다.
실제 채점 로직 구현은 Company Instance의 `scripts/`에서 수행한다 (Phase 1 Mock Vertical Slice 참고).
