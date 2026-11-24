원본: [[inbox/2026-01-13부터 2026-06-25 상반기 소모임 기록]], [[inbox/2026-07-09 CLI 기반 AI 에이전트 입문과 핵심 개념]], [[inbox/2026-08-06 MCP와 스킬로 업무도구 연결 실습]]

우리 소모임의 주력 하네스 3종은 Claude Code, Codex, Antigravity이며, 각자의 AI 구독 상태와 선호하는 화면(터미널 vs 앱)에 맞춰 골라 쓴다.

## 1. 구독별 하네스 3종 비교 및 설치법 (7/9 실습 기준)

| 도구 이름 | 필요한 구독 | 형태 및 특징 | 설치 방법 |
|---|---|---|---|
| **Claude Code** | Claude 유료 (Pro 이상) | 터미널 CLI 및 Claude 데스크톱 앱 Code 탭. `claude.ai` 커넥터(Slack·캘린더·monday) 공유 지원 | • Mac: `curl -fsSL https://claude.ai/install.sh \| bash`<br>• Win(PowerShell): `irm https://claude.ai/install.ps1 \| iex` |
| **Codex** | ChatGPT 유료 (Plus 이상) | 터미널 CLI 및 ChatGPT 데스크톱 앱 Codex. `AGENTS.md`와 `.agents/skills/` 기본 인식 | • Mac: `curl -fsSL https://chatgpt.com/codex/install.sh \| sh`<br>• Win(PowerShell): `powershell -ExecutionPolicy ByPass -c "irm https://chatgpt.com/codex/install.ps1 \| iex"` |
| **Antigravity** | Gemini 또는 무구독 가능 | VS Code 스타일 앱 및 CLI(`agy`). 구글 계정만 있으면 프리뷰 사용 가능, 계획서(Plan)를 먼저 보여주는 화면 제공 | • `antigravity.google/download`에서 OS별 설치 파일 다운로드 |

## 2. 시기별 변화 및 참고사항 (시점 대조)
- **2026-02-24 소모임 당시**: Antigravity에서 바이브 코딩 실습을 할 때 에이전트 엔진으로 `Gemini Pro(low)`만 동작하고 `high`는 동작하지 않았다.
- **2026-07-09 · 2026-08-06 이후**: 3종 도구 모두 MCP 서버(`korea-rio-mcp` 등)와 스킬(`SKILL.md`) 연동이 표준화되었으며, 8/6 소모임에서는 똑같은 실습 주제를 Claude Code, Codex, Antigravity 세 에이전트가 각각 생성한 슬라이드와 진행안으로 비교했다.

## 3. 자주 겪는 설치 트러블슈팅
- **`command not found` 오류**: 설치 직후에는 터미널(또는 PowerShell)을 완전히 껐다가 다시 열어야 경로(`PATH`)가 반영된다.
- **Claude Code 로그인 거부**: 무료 플랜 계정일 때 발생하므로 Claude Pro 이상을 구독하거나 Antigravity로 전환한다.

## 4. 연결된 노트
- [[개념 - 하네스와 CLI 에이전트]] — 세 도구가 공통으로 따르는 하네스·CLI 기본 개념이다.
- [[개념 - MCP와 스킬과 플러그인]] — 세 도구에 똑같은 MCP 서버와 `SKILL.md`를 심링크로 연결해 쓰는 방법이다.
