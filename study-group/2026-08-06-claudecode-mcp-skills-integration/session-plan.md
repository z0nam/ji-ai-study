# MCP로 그룹웨어/슬랙/메일 연결 및 나만의 스킬(Skill) 만들기 실습 — Claude Code 판
**제주연구원 인공지능연구회 소모임 진행안**

- **일시**: 2026년 8월 6일(목) 17:00–18:00 (60분)
- **장소**: 노꼬메 회의실
- **준비물**: 개인 노트북 (Claude Code 또는 Codex CLI 1개 이상 미리 설치 + 로그인)
- **목표**:
  1. 세 가지 연결 방식(**커넥터 / 로컬 MCP 서버 / SKILL.md**)을 직접 손으로 붙여보기
  2. 우리 github 공개 자산을 내 노트북에 얹어 AI의 손발로 쓰기
  3. 반복 업무를 자동화하는 나만의 Custom Skill(`SKILL.md`) 1개 만들기

> 이 진행안은 [Antigravity 생성판](../2026-08-06-antigravity-mcp-skills-integration/session-plan.md)과 병존하는 Claude Code 생성판입니다.
> 핵심 차이: Slack은 저장소 클론이 아니라 **플러그인/커넥터**로 붙이고, MCP 정석 실습은 크로스플랫폼 **`korea-rio-mcp`**, 스킬 본보기는 **`messages-cli`**를 씁니다.

---

## 세 가지 연결 방식 (오늘의 뼈대)

| 방식 | 붙이는 법 | 오늘 실습 |
|---|---|---|
| **커넥터** | claude.ai Connectors에서 스위치 ON → Claude Code 공유 | Slack · monday · 캘린더 |
| **로컬 MCP 서버** | 프로그램을 stdio로 감싸 `claude mcp add` | korea-rio-mcp |
| **SKILL.md + 심링크** | PATH에 CLI + 사용설명서 1장 심링크 | 나만의 스킬, messages/mail/gw |

---

## 0. 세션 직전 체크리스트 (진행자)

- [ ] 시연 노트북: Slack·monday 커넥터, `korea-rio-mcp`, `mail`/`gw` 스킬 세팅 확인
- [ ] `claude mcp list` → 붙인 서버가 전부 `✔ Connected` 인지 확인
- [ ] 노꼬메 회의실 프로젝터 / 화면 공유 테스트 (라이브 슬라이드 링크 열어두기)
- [ ] **그룹웨어(gw)는 패턴 시연만** — 화면에 `.env`·내부망 IP·자격증명 절대 노출 금지
- [ ] 참가자 안내: 맥/윈도우 혼재 → "모두 따라하기" 축은 **Slack·korea-rio-mcp·나만의 스킬**

---

## 1부. 개념: MCP & Skill (17:00–17:12, 12분)

### 핵심 개념
1. **MCP = AI에 도구를 꽂는 "표준 플러그"** — USB처럼 규격을 한 번 맞추면 Claude·Codex·Gemini 어디서든 같은 도구가 꽂히고, AI가 그 도구를 함수처럼 호출한다.
2. **Skill = "이 도구 언제·어떻게 쓰나" 사용설명서(`SKILL.md`)** — CLI/셸 명령의 사용법과 안전수칙을 적은 AI 전용 매뉴얼.
3. **두 패턴**
   - **SKILL.md 패턴**: PATH에 CLI 깔고 `SKILL.md` 한 장 심링크 → AI가 알아서 `Bash`로 호출 (messages·contacts·gw·mail)
   - **MCP 서버 패턴**: 프로그램을 stdio로 감싸 `claude mcp add` → AI가 툴로 직접 호출 (korea-rio-mcp)

---

## 2부. 실습 A — 커넥터로 붙이기: Slack·monday·캘린더 (17:12–17:25, 13분)

### ① Slack (모두 따라하기)
**길 1 — 플러그인**
```
/plugin marketplace add anthropics/claude-plugins-official
/plugin install slack@claude-plugins-official
```
설치 후 OAuth 인증 → 채널 읽기·검색·보내기 툴 활성.

**길 2 — 커넥터**: claude.ai → Settings → Connectors에서 **Slack 켜기** → Claude Code와 자동 공유(2.1.46+).

- 확인 프롬프트: `"슬랙 #인공지능연구회소모임 최근 글 3줄로 요약해줘"`
- 회사 슬랙 채널ID·유저UID는 `slack-directory` 스킬로 로컬 캐시 후 `grep`(API 호출 절약).

### ② monday · 캘린더 (클릭)
- claude.ai Connectors에서 **Google Calendar · monday.com** 켜기 → Claude Code 공유.
- monday 로컬 등록도 가능: `claude mcp add monday …` 또는 `.mcp.json`.
- 실사례 — 이 커넥터들을 **조합**한 오케스트레이션 스킬 `calendar-worklog`:
```
/plugin marketplace add z0nam/calendar-worklog
/plugin install calendar-worklog-ji@calendar-worklog
# 이후: /worklog day | week | now
```

---

## 3부. 실습 B·C — 그룹웨어와 메일 (17:25–17:38, 13분)

### ③ 그룹웨어 (gw · 사내 사례, 패턴 시연)
레거시 그룹웨어를 **CLI+어댑터**로 감싸고 `SKILL.md`로 노출 (MCP 아님). 우리 GW에 맞춰 어댑터만 갈면 확장.
```
gw approvals              # 결재함 읽기
gw download <wscNo> --dm  # 결재 본문 → Slack DM
gw late-check --draft     # 출근기록 대조 → 지참 기안(상신 직전 정지)
gw budget                 # 과제 예산 잔액
```
- 포인트: 결재 알림이 **Slack DM으로** → 그룹웨어↔슬랙이 한 흐름.
- **상신 같은 위험 행위는 사람이** (기안 직전 정지). 자격증명·내부망은 화면 밖.

### ④ 메일 (mail-skill · 공개, 백엔드 중립)
엔진(Himalaya)을 어댑터 뒤에 숨긴 **5-verb 계약**. 엔진을 바꿔도 `bin/mailskill` 한 파일만 고치면 사용법은 그대로.
```
mailskill accounts
mailskill list   <acct> 10
mailskill search <acct> <query>
mailskill read   <acct> <id>
mailskill reply  <acct> <id>   # 초안만, 발송은 승인 후
```
- 설치 개요: `brew install himalaya age` → `scripts/bootstrap.sh`(age키+심링크) → 계정 앱비번 등록.
- 3갈래 라우팅: 주 Gmail=커넥터, 그 외 IMAP=어댑터, 과거메일=로컬 Maildir. 시크릿은 age 암호화(평문 없음).

---

## 4부. 실습 D — MCP 서버 직접 꽂기 (17:38–17:47, 9분)

**`korea-rio-mcp`** (공개, 맥·윈도우 공통) — 지역 경제파급효과를 계산하는 크로스플랫폼 MCP 서버. 서버 하나가 세 플랫폼을 먹는다.

**Claude Code**
```
uv tool install git+https://github.com/z0nam/korea-rio-mcp
claude mcp add korea-rio -- korea-rio-mcp
```
**Codex** (`~/.codex/config.toml`)
```toml
[mcp_servers.korea_rio]
command = "korea-rio-mcp"
```
**Gemini CLI** (`~/.gemini/settings.json`)
```json
{ "mcpServers": { "korea-rio": { "command": "korea-rio-mcp" } } }
```
- 확인: `claude mcp list` → `✔ Connected`.
- 프롬프트: `"서귀포에 100억 투자하면 생산유발효과가 얼마야?"`

---

## 5부. 실습 E — 나만의 스킬 만들기 ★ 라이브 (17:47–17:55, 8분)

본보기: `messages-cli`. 스킬 = **frontmatter(언제 쓰는지)** + 본문(명령/안전수칙).

`~/dev/mytool/.claude/skills/mytool/SKILL.md`:
```markdown
---
name: mytool
description: 무슨 일을 하는지 + "언제 이 스킬을 쓰는지" 트리거 문장.
  "OO 해줘" 같은 발화가 오면 먼저 이 스킬을 본다.
---

# mytool
## 명령
mytool run <arg>   # 하는 일
## 안전수칙
쓰기·삭제·전송은 사용자 승인 후.
```

심링크 = SSOT (한 원본을 Claude·Codex 양쪽에):
```bash
# CLI를 PATH에
ln -sf "$PWD/mytool" ~/.local/bin/mytool
# SKILL.md 하나를 양쪽 에이전트에
ln -sf "$PWD/.claude/skills/mytool" ~/.claude/skills/mytool
ln -sf "$PWD/.claude/skills/mytool" ~/.codex/skills/mytool
```
- 핵심: `description`에 "이럴 때 쓴다"를 잘 써야 AI가 알아서 집어든다.
- 원본 **한 곳만** 고치면 Claude·Codex 양쪽 반영 → 두 에이전트 동시 지원의 비결.

---

## 6부. Q&A · 배포 원칙 (17:55–18:00, 5분)

**팀에 나눌 땐 두 원칙** (사내 `ji-deploy-standards`)
1. **플랫폼 프리** — AI 구독만 있으면 Claude·Codex·Gemini 동일. 로컬 stdio MCP 하나 = 데스크톱·CLI 넷 커버.
2. **"설치 수준" UX** — 컴맹도 프로그램 설치 한 번 경험으로. 등록은 `setup` 명령이 각 config에 멱등 주입.
- 소비자 챗앱(ChatGPT·Gemini앱)은 로컬 MCP 불가 → 자체호스팅 웹링크(HTTP)로 우회.

**안전 수칙**
1. 토큰·비밀번호는 코드에 하드코딩 금지, `.env` 전역 관리.
2. 삭제·상신·전송 등 변경 작업은 에이전트 확인(승인) 거쳐 실행.
3. **다음 숙제**: 내 반복 업무 명령 1개를 골라 `SKILL.md`로 만들어 오기.

---

## 트러블슈팅 가이드

| 증상 | 원인 | 해결책 |
|---|---|---|
| `<tool>: command not found` | PATH 미등록 | `~/.zshrc`에 `export PATH="$HOME/.local/bin:$PATH"` 후 터미널 재실행 |
| Slack 툴 안 뜸 | 플러그인 미설치/미인증 | `/plugin install slack@claude-plugins-official` 후 인증, 또는 claude.ai Connectors에서 Slack 재인증 |
| `claude mcp list`에 서버 없음 | `claude mcp add` 미실행 or 실행파일 PATH 밖 | 콘솔스크립트 절대경로로 등록하거나 `uv tool install` 재실행 |
| korea-rio-mcp 연결 실패 | `korea-rio-mcp` PATH 밖 | 절대경로 사용 또는 `python -m rio_mcp.server` |
| 스킬 인식 안 됨 | 심링크 경로 오류 | `~/.claude/skills/` · `~/.codex/skills/`에 심링크 걸렸는지 확인 |
| mailskill 계정 오류 | 앱비번 미등록 | `scripts/secret-set himalaya-<name>`로 계정 앱비번 등록 |
