# ADR-002: GBrain 연동 방식

**날짜**: 2026-06  
**상태**: 제안됨 (Proposed)  
**관련**: ADR-001 (Source of Truth 분리)

## 배경

GBrain을 조직의 장기 기억 엔진으로 사용하기로 했다. Phase 1은 Mock Adapter로 동작했고, 실제 연결 전에 공식 인터페이스를 확인해야 한다 (추측 금지 원칙).

## 조사 결과 (공식 문서 기반)

출처: [garrytan/gbrain](https://github.com/garrytan/gbrain) 공식 README 및 company-brain 튜토리얼 (2026-06 확인)

### GBrain의 핵심 성격

- **Markdown-first**: 지식은 git "brain repo"의 Markdown 파일로 존재하고, GBrain이 이를 Postgres로 sync한다. git 삭제는 DB에서 soft-delete로 보존된다.
- **두 엔진**: PGLite(로컬, ~50K 페이지 기본) / PostgreSQL+pgvector(Supabase, 공유·다중). `gbrain migrate --to supabase`
- **MCP 서버**: `gbrain serve --http` — OAuth 2.1 + 30+ MCP tools를 Claude Code에 직접 노출
- **SQL 레벨 격리**: source-scoped OAuth, "cross-source read를 SQL 계층에서 거부" — DB가 격리 강제

### 주요 작업 (CLI / MCP)

| 구분 | 명령 | 용도 |
|---|---|---|
| 읽기 | `gbrain search "<q>"` | 하이브리드 검색 (vector + BM25 + RRF), LLM 비용 없음 |
| 읽기 | `gbrain think "<q>"` | 인용 + Gap 분석 포함 합성 답변 |
| 읽기 | `gbrain graph-query` | 다중 홉 그래프 탐색 |
| 쓰기 | `gbrain capture` / `put_page` | 페이지 생성 (엔티티 자동 링크, LLM 호출 0) |
| 쓰기 | `gbrain import` | 파일시스템 일괄 수집 |
| 운영 | `gbrain sync --all`, `gbrain doctor`, `gbrain autopilot` | 동기화/유지보수/백그라운드 |

### brain repo 구조

```
shared/        # 조직 공통
customers/     # 고객별 (source-scoped)
internal/      # 법무·HR 등
```

Markdown + YAML frontmatter, `[[wikilinks]]` 엔티티 참조, typed-link 엣지(`works_at`, `invested_in` 등).
워크스페이스 repo는 `crons/`, `skills/`, `_brain-filing-rules.md` 등 운영 규칙을 둔다.

## 결정

1. **GBrain을 재구현하지 않는다.** 공식 CLI/MCP를 그대로 사용한다 (지시서 원칙 2.3).
2. **Claude Code ↔ GBrain은 HTTP MCP로 직접 연결한다.** 커스텀 REST 어댑터를 만들지 않는다.
   - `GBrainAdapter` TS 인터페이스는 Mock·테스트 및 타입 계약용으로 유지한다.
   - 실제 런타임에서는 Claude Code가 GBrain MCP tool(`search`, `think`, `capture`)을 직접 호출한다.
3. **brain repo = 회사별 source.** 우리 `company-a-operator/brain/`을 GBrain source로 등록하고 Supabase로 sync한다.
4. **회사 격리는 GBrain의 source-scoped OAuth + SQL 격리로 강제한다.** 회사 A/B/C는 각각 별도 source(또는 별도 Supabase 프로젝트)를 가진다 — 프롬프트만으로 격리하지 않는다.

## 스키마 매핑

| 우리 모델 | GBrain 표현 |
|---|---|
| Project Episode | `projects/` 또는 `episodes/` source의 Markdown 페이지 (frontmatter + wikilinks) |
| queryEpisodes() | `gbrain search` / `think` (read scope) |
| saveEpisode() | `gbrain capture` / `put_page` (**write scope — 사람 승인 후에만**) |
| 회사 격리 | source-scoped OAuth client + SQL 강제 격리 |

## 비용·데이터 경로 (정책상 필수 문서화)

GBrain은 임베딩 제공자가 필요하다 (ZeroEntropy 기본 / OpenAI / Voyage / **Ollama·llama.cpp 로컬**).

- 회사 기밀을 외부 임베딩 API로 보내는 것은 `policies/data-handling-policy.yaml`의 `external-api-approval`·`cost-documentation`에 해당 → **사람 승인 + 비용·경로 문서화 필수**.
- 기밀 데이터는 **로컬 임베딩(Ollama)** 우선 검토. 외부 제공자 사용 시 비용·전송 경로를 연결 전에 문서화한다.

## 결과

- Mock Adapter는 그대로 두고, 실제 연결은 MCP로 전환한다 (인터페이스 동일하므로 오케스트레이션 로직 불변).
- GBrain은 ~v0.30으로 breaking change가 잦다 → **설치 버전을 고정**하고, 실제 노출된 MCP tool 이름을 설치 후 확인한 뒤 구현한다.

## 재검토 조건

- GBrain MCP tool 이름/스코프가 버전업으로 바뀌면 재검토.
- 로컬 임베딩 품질이 부족하면 vetted 외부 제공자 도입을 비용 문서화 후 재검토.
- 회사가 늘어 Supabase 프로젝트 분리 전략이 필요하면 company-isolation 문서와 함께 재검토.
