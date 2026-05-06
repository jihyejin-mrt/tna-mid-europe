# tna-mid-europe — 마이리얼트립 중부유럽팀 Claude Code 플러그인

마이리얼트립 Tour & Activities 사업부 중부유럽팀 공용 Claude Code 슬래시 커맨드 모음입니다. 이슈 드리븐 분석, 파트너 이메일, 회의록 정리, KPI 리포트, 시장 조사, 파트너 리서치를 한 번에 처리할 수 있도록 팀의 작업 원칙(두괄식 · 8단계 분석 · MECE · A/B/C 다음 선택지)을 내장했습니다.

## 빠른 시작 (팀원용)

**1단계 — API 키 발급 후 환경변수 설정** (`/market`·`/partner`·`/kpi`가 실제 데이터를 가져오려면 필수):

```bash
# ~/.zshrc 또는 ~/.zshenv 에 추가 후 source ~/.zshrc
export TAVILY_API_KEY="<본인-Tavily-키-여기에>"     # 발급: https://app.tavily.com → API Keys
export REDASH_API_KEY="<본인-Redash-토큰-여기에>"   # 발급: https://redash.myrealtrip.net → User Settings → API Key
# REDASH_URL은 기본값(https://redash.myrealtrip.net)이 자동 적용됨
```

> Tavily는 무료 플랜으로 월 1,000 검색까지 가능합니다. Redash는 사내 계정 기반이므로 본인 토큰을 발급하세요.

**2단계 — Claude Code에서 마켓플레이스 등록 + 플러그인 설치**:

```bash
/plugin marketplace add jihyejin-mrt/tna-mid-europe
/plugin install tna-europe-toolkit@mrt-tna-europe
```

**3단계 — Claude Code 재시작.** MCP 서버 연결을 위해 한 번 재시작이 필요합니다. 재시작 후 `claude mcp list`에 `tavily`·`redash`가 `✓ Connected`로 보이면 정상.

설치가 끝나면 아래 6개 슬래시 커맨드 + 2개 MCP 서버가 함께 활성화됩니다.

## 제공 커맨드

| 커맨드 | 용도 | 입력 예시 |
|--------|------|----------|
| `/tna-europe-toolkit:analyze` | 이슈 드리븐 8단계 분석 (空雨傘) | `/tna-europe-toolkit:analyze 베를린 워킹투어 GMV가 3주째 감소 중인 이유` |
| `/tna-europe-toolkit:email` | 파트너사 이메일 영문/국문 초안 | `/tna-europe-toolkit:email Tour Berlin GmbH에 신규 입점 제안, 할인율 협상 포함` |
| `/tna-europe-toolkit:kpi` | KPI 현황 리포트 + 원인 분석 | `/tna-europe-toolkit:kpi 2026-W18 con_margin 달성률 85%, 전주 대비 -8%` |
| `/tna-europe-toolkit:market` | 유럽 T&A 시장 조사 (3C) | `/tna-europe-toolkit:market 프라하 푸드투어 시장 규모와 GYG 점유율` |
| `/tna-europe-toolkit:meeting` | 회의록 정리 + 액션아이템 추출 | `/tna-europe-toolkit:meeting [회의록 원문 붙여넣기]` |
| `/tna-europe-toolkit:partner` | 신규 파트너 리서치 + 영업 준비 | `/tna-europe-toolkit:partner Vienna Insider Tours 입점 평가` |

> 💡 자동완성에서 `/tna`만 쳐도 6개 커맨드가 모두 필터링되어 보입니다.

각 커맨드는 두괄식 결론 → 분석 본문 → A/B/C 다음 선택지 형식으로 출력합니다.

## MCP 서버 (Tavily · Redash, 플러그인에 동봉됨)

플러그인 설치 시 두 MCP 서버가 자동으로 함께 등록됩니다 — 별도 `.mcp.json` 작성 불필요.

| MCP | 용도 | 사용하는 커맨드 | API 키 발급 |
|-----|------|----------------|-------------|
| `tavily` | 실시간 웹 검색 (시장·경쟁사·파트너 정보) | `/market`, `/partner` | https://app.tavily.com |
| `redash` | 사내 데이터 쿼리 (`con_margin`, `GMV` 등) | `/kpi`, `/analyze` | https://redash.myrealtrip.net → User Settings → API Key |

API 키는 보안상 플러그인에 포함하지 않고 **환경변수**로 주입합니다. 위 "빠른 시작 1단계"의 `export` 가이드를 따르세요.

### 동작 확인

```bash
# Claude Code 재시작 후
claude mcp list
# → tavily: ✓ Connected, redash: ✓ Connected 가 보여야 정상
```

연결 실패 시 체크 포인트:
1. `echo $TAVILY_API_KEY` 로 환경변수가 실제로 export 되었는지
2. Claude Code를 새 터미널에서 재시작했는지 (기존 세션은 환경변수 변경을 못 봄)
3. `npx`가 PATH에 있는지 (`which npx`) — Node.js 미설치 시 `brew install node`

## 팀 작업 원칙 (선택: 항상 적용하기)

플러그인 슬래시 커맨드 안에는 핵심 출력 규칙(두괄식, 굵은 글씨, A/B/C 선택지)이 이미 박혀 있어서 커맨드 호출 시 자동 적용됩니다. **다른 일반 대화에도 같은 원칙을 적용**하고 싶다면 [`CLAUDE.md`](./CLAUDE.md)의 "Claude 작업 원칙" 섹션을 본인 `~/.claude/CLAUDE.md`에 복사해 두세요.

핵심 원칙 요약:

- **0번**: 모르면 추측하지 말고 물어본다
- **두괄식**: 결론을 첫 문장에 명시
- **MECE**: 중복 없이, 빠뜨림 없이
- **이슈 드리븐**: 올바른 답보다 올바른 질문
- **80/20**: 임팩트 큰 20%에 집중
- **A/B/C 선택지**: 분석·제안 후 다음 행동 3개 제시

전체 원칙은 `CLAUDE.md`를 참고하세요.

## 메인테이너용 — 로컬 개발 / 테스트

이 저장소를 직접 클론한 경우 (메인테이너) 로컬 마켓플레이스로도 등록할 수 있습니다:

```bash
# 저장소 루트에서
/plugin marketplace add ./

# 변경사항 푸시 후 팀원이 받으려면
/plugin marketplace update mrt-tna-europe
```

### 디렉토리 구조

```
tna-mid-europe/
├── .claude-plugin/
│   └── marketplace.json           # 마켓플레이스 매니페스트
├── plugins/
│   └── tna-europe-toolkit/
│       ├── .claude-plugin/
│       │   └── plugin.json        # 플러그인 매니페스트
│       ├── .mcp.json              # Tavily + Redash MCP (env var 참조)
│       └── commands/              # 6개 슬래시 커맨드
│           ├── analyze.md
│           ├── email.md
│           ├── kpi.md
│           ├── market.md
│           ├── meeting.md
│           └── partner.md
├── CLAUDE.md                      # 팀 작업 원칙 (참조용)
├── .mcp.json.example              # 플러그인 미사용 시 참고용 템플릿
├── .pre-commit-config.yaml        # detect-secrets 훅
└── README.md
```

### 새 커맨드 추가하기

1. `plugins/tna-europe-toolkit/commands/<이름>.md` 생성
2. frontmatter에 `description`, `argument-hint` 작성
3. 본문에 `$ARGUMENTS` 자리표시자 사용
4. 출력은 항상 **두괄식 결론 → 본문 → A/B/C 다음 선택지** 구조 유지
5. `plugins/tna-europe-toolkit/.claude-plugin/plugin.json`의 `version` bump
6. 커밋·푸시 → 팀원은 `/plugin marketplace update mrt-tna-europe`

## 라이선스

사내용 — 마이리얼트립 중부유럽팀 (T&A) 공용.

## 문의

- Slack (유럽팀): [#europe](https://myrealtrip.slack.com/archives/C04KAEKNG0L)
- Confluence (유럽팀): [Europe Space](https://myrealtrip.atlassian.net/wiki/spaces/TA/pages/3325919465/Europe)
- 메인테이너: jihye.jin@myrealtrip.com
