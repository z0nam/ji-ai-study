# 2026-08-06 소모임: MCP로 그룹웨어/슬랙/메일 연결 및 스킬(Skill) 실습 — Claude Code 판

8월 6일(목) 17:00~18:00 노꼬메 회의실에서 진행된 인공지능연구회 소모임 실습 자료입니다.
같은 주제로 [Antigravity가 생성한 자료](../2026-08-06-antigravity-mcp-skills-integration)와 **병존**하는 Claude Code 생성판으로, 두 에이전트의 결과물을 비교해볼 수 있습니다.

## 📌 주요 내용
세 가지 **연결 방식**을 손으로 만져보는 흐름:
- **커넥터(클릭)** — claude.ai Connectors에서 Slack·Gmail·캘린더·monday 켜면 Claude Code까지 공유
- **로컬 MCP 서버(`claude mcp add`)** — `korea-rio-mcp`를 Claude·Codex·Gemini 세 플랫폼에 등록
- **SKILL.md + 심링크** — CLI 하나 + 사용설명서 한 장으로 나만의 스킬 만들기

실습 자산은 전부 우리 github(z0nam)에 공개/사내로 올라가 있습니다: `mail-skill`, `messages-cli`, `contacts-cli`, `korea-rio-mcp`, `calendar-worklog`, (사내) `ji-gw-ai`, `ji-deploy-standards`.

## 📂 파일 및 라이브 슬라이드
- 🌐 **[발표 슬라이드 바로보기 (GitHub Pages)](https://z0nam.github.io/ji-ai-study/study-group/2026-08-06-claudecode-mcp-skills-integration/slides.html)** — 브라우저에서 바로 열리는 웹 슬라이드 (방향키 `↓ ↑`로 넘김, 코드블록 COPY 버튼)
- 🌐 **[HTML Preview 링크 (대체)](https://htmlpreview.github.io/?https://github.com/z0nam/ji-ai-study/blob/main/study-group/2026-08-06-claudecode-mcp-skills-integration/slides.html)** — 소스 대신 렌더링 화면 열기
- [`slides.html`](./slides.html) — 13장 웹 슬라이드 소스 (독립 실행형, 의존성 0)
- [`session-plan.md`](./session-plan.md) — 상세 세션 진행안 및 트러블슈팅 가이드

---
*운영: 제주연구원 인공지능연구회 · 생성: Claude Code*
