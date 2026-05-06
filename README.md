# tna-mid-europe — 마이리얼트립 중부유럽팀 Claude Code 플러그인

마이리얼트립 Tour & Activities 사업부 중부유럽팀 공용 Claude Code 슬래시 커맨드 모음입니다. 이슈 드리븐 분석, 파트너 이메일, 회의록 정리, KPI 리포트, 시장 조사, 파트너 리서치를 한 번에 처리할 수 있도록 팀의 작업 원칙(두괄식 · 8단계 분석 · MECE · A/B/C 다음 선택지)을 내장했습니다.

## 빠른 시작 (팀원용)

Claude Code에서 아래 두 줄만 실행하세요:

```bash
# 1. 이 마켓플레이스를 등록
/plugin marketplace add jihyejin-mrt/tna-mid-europe

# 2. 플러그인 설치
/plugin install tna-europe-toolkit@mrt-tna-europe
```

설치가 끝나면 아래 6개 슬래시 커맨드가 바로 사용 가능합니다.

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

## MCP 서버 설정 (Tavily / Redash)

`/market`, `/partner`는 Tavily 웹 검색을 활용하고, `/kpi`는 Redash 쿼리를 참조하기 때문에 두 MCP 서버 연결을 권장합니다.

1. 저장소의 `.mcp.json.example`을 본인 Claude 설정 위치(`~/.claude.json` 또는 프로젝트 루트의 `.mcp.json`)에 복사
2. API 키를 본인 것으로 교체:
   - **Tavily**: https://app.tavily.com 가입 → API Keys → 발급 (`tvly-...`)
   - **Redash**: https://redash.myrealtrip.net → 우상단 프로필 → User Settings → API Key
3. Claude Code 재시작

> **주의**: `.mcp.json`은 절대 깃에 커밋하지 마세요. 본 저장소는 `detect-secrets` pre-commit hook으로 보호되고 있으며 `.gitignore`에 `.mcp.json`이 등록되어 있습니다.

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
│       └── commands/              # 6개 슬래시 커맨드
│           ├── analyze.md
│           ├── email.md
│           ├── kpi.md
│           ├── market.md
│           ├── meeting.md
│           └── partner.md
├── CLAUDE.md                      # 팀 작업 원칙 (참조용)
├── .mcp.json.example              # MCP 서버 설정 템플릿
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
