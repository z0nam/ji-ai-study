원본: [[inbox/2026-07-09 CLI 기반 AI 에이전트 입문과 핵심 개념]], [[inbox/2026-08-06 MCP와 스킬로 업무도구 연결 실습]]

MCP는 외부 시스템에 들어가는 표준 출입증이고, 스킬(`SKILL.md`)은 우리 방식대로 일하게 만드는 한국어 업무 매뉴얼이며, 플러그인은 이들을 하나로 묶은 세트 상자다.

## 1. MCP (Model Context Protocol) — 사내 시스템 출입증
- AI가 외부 프로그램이나 서비스(슬랙, 그룹웨어, 한글 문서, 구글 캘린더 등)와 연결되는 표준 규격으로 **"AI용 USB-C 단자"**에 해당한다.
- 한 번 표준 규격으로 만든 MCP 서버는 Claude Code, Codex, Antigravity 어디에나 똑같이 꽂아 쓸 수 있다.

## 2. 스킬 (Skill) — 업무 매뉴얼 폴더 (`SKILL.md`)
- 특정 작업의 절차·요령·안전수칙을 한국어로 적어둔 **`SKILL.md` 파일이 든 폴더**다.
- **MCP와의 차이**: MCP가 그룹웨어에 `"들어갈 수 있게"` 해주는 출입증이라면, 스킬은 들어가서 `"우리 기관 양식과 규칙대로 일하게"` 해주는 매뉴얼이다.
- 스킬 만들기는 코딩이 아니라 한국어로 매뉴얼을 쓰는 일이며, 에이전트에게 `"내가 방금 한 작업 과정을 스킬로 저장해줘"`라고 시켜도 된다.
- `SKILL.md` 맨 위 frontmatter의 `description`에 **"언제 이 스킬을 쓰는지"** 트리거 문장을 명확히 적어야 에이전트가 알아서 집어 든다.

## 3. 에이전트에 도구와 스킬을 붙이는 3가지 실전 방식 (8/6 실습)
1. **커넥터 (클릭 / 플러그인 설치)**:
   - `claude.ai` Settings → Connectors에서 Slack·monday.com·Google Calendar 스위치를 켜거나 `/plugin install`로 설치하면 Claude Code까지 자동으로 공유된다.
2. **로컬 MCP 서버 (`mcp add`)**:
   - 로컬 프로그램을 stdio로 감싸 에이전트에 등록한다.
   - 예: 지역 경제파급효과를 계산하는 `korea-rio-mcp`를 설치(`uv tool install git+https://github.com/z0nam/korea-rio-mcp`)한 뒤 `claude mcp add korea-rio -- korea-rio-mcp`로 꽂으면 세 플랫폼 모두에서 쓸 수 있다.
3. **SKILL.md + 심링크 (SSOT 패턴)**:
   - 실행할 CLI 도구를 `PATH`(`~/.local/bin`)에 두고, 사용설명서 `SKILL.md` 한 장을 `~/.claude/skills/`와 `~/.codex/skills/` 양쪽에 심링크(`ln -sf`)로 연결한다.
   - 원본 한 곳만 고치면 모든 에이전트에 동시에 반영된다.

## 4. AGENTS.md / CLAUDE.md와의 차이
- **스킬(`SKILL.md`)**: 특정 작업을 만났을 때만 꺼내 읽는 개별 작업 매뉴얼.
- **`AGENTS.md` / `CLAUDE.md`**: 프로젝트 폴더 맨 위에 두어 대화를 시작할 때마다 **항상** 자동으로 읽는 팀 기본 규칙 게시판(Claude Code는 `CLAUDE.md`, Codex와 Antigravity는 `AGENTS.md`를 읽으며 `@AGENTS.md`로 한 줄 연결 가능).

## 5. 연결된 노트
- [[개념 - 하네스와 CLI 에이전트]] — 에이전트 5대 개념의 전체 그림과 이어진다.
- [[우리자산 - JAIX kit와 제주연구원 업무 스킬]] — 우리 연구원에서 실제로 만들어 쓰는 MCP·스킬 자산 목록과 연결된다.
