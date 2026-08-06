# MCP로 그룹웨어/슬랙/먼데이/메일 연결 및 나만의 스킬(Skill) 만들기 실습
**제주연구원 인공지능연구회 소모임 진행안**

- **일시**: 2026년 8월 6일(목) 17:00–18:00 (60분)
- **장소**: 노꼬메 회의실
- **준비물**: 개인 노트북 (Claude Code / Codex / Antigravity 중 1개 이상 미리 설치)
- **목표**: 
  1. GitHub의 4대 연결 자산(`ji-gw-ai`, `ji-slack-admin`, `ji-monday-admin`, `mail-skill`)을 내 컴퓨터에 복사/설정하기
  2. 에이전트(Claude Code / Antigravity / Codex)에 MCP 및 스킬(Skill)을 연동하여 직접 업무에 사용해보기
  3. 반복 업무를 자동화하는 나만의 Custom Skill (`SKILL.md`) 1개 직접 만들기

---

## 0. 세션 직전 체크리스트 (진행자)

- [ ] 시연용 노트북: `gw`, `mail`, `slack`, `monday` MCP/스킬 4종 세팅 완료 확인
- [ ] 노꼬메 회의실 프로젝터 / 화면 공유 테스트
- [ ] 자산 저장소 접근 확인: `github.com/z0nam/ji-gw-ai`, `ji-slack-admin`, `ji-monday-admin`, `mail-skill`
- [ ] `.env.example` 템플릿 파일이 포함되어 있는지 확인 (토큰/비밀번호 노출 안 됨)
- [ ] 실습용 빈 폴더 생성 안내 준비 (`~/dev/ai-workspace` 등)

---

## 1부. 개념: MCP & Skill로 내 컴퓨터와 사내 도구 연결하기 (17:00–17:10, 10분)

### 핵심 개념 정리
1. **MCP (Model Context Protocol)**:
   - AI 에이전트가 외부 서비스(슬랙, 먼데이, 메일, DB)의 데이터와 API를 실시간 표준 규격으로 주고받는 **"사내 시스템 출입증 & 실시간 API 통로"**
2. **Skill (스킬)**:
   - CLI 도구나 셸 명령, Python 스크립트 실행 절차와 규칙을 정의한 **"AI 전용 업무 매뉴얼 (`SKILL.md`)"**
3. **GitHub 자산 활용 목적**:
   - 이미 만들어진 사내 자산 4종(`ji-gw-ai`, `ji-slack-admin`, `ji-monday-admin`, `mail-skill`)을 각자의 노트북에 가져와 곧바로 내 AI 에이전트의 손발로 탑재!

---

## 2부. 실습 1단계: GitHub 자산 내 컴퓨터로 가져오기 & 설정 (17:10–17:25, 15분)

### 1) 저장소 클론 및 패키지 설치
각자의 터미널에서 작업 폴더로 이동 후 필수 패키지 준비:
```bash
# 기본 작업 폴더 생성
mkdir -p ~/dev && cd ~/dev

# 1. 그룹웨어 제어 도구 (ji-gw-ai)
git clone https://github.com/z0nam/ji-gw-ai.git
cd ji-gw-ai && uv sync && cd ..

# 2. 슬랙 운영 도구 (ji-slack-admin)
git clone https://github.com/z0nam/ji-slack-admin.git

# 3. 먼데이 운영 도구 (ji-monday-admin)
git clone https://github.com/z0nam/ji-monday-admin.git

# 4. 메일 스킬 (mail-skill)
git clone https://github.com/z0nam/mail-skill.git
```

### 2) `.env` 자격증명(Credentials) 설정
- `cp .env.example .env` 후 본인의 로그인 정보/API 토큰 입력
- `.env`는 `.gitignore` 처리되어 있어 절대로 깃허브에 올라가지 않음!

---

## 3부. 4대 서비스 직접 연동하고 써보기 (17:25–17:45, 20분)

### ① 메일 (Mail Skill) 연동
- **프롬프트 예시**: `"최근 내 메일함에서 '소모임' 관련 메일 찾아서 3줄 요약해줘"`
- **동작**: Local IMAP / Gmail 연동 스킬이 메일을 조회하여 핵심 내용 출력

### ② 슬랙 (Slack MCP) 연동
- **프롬프트 예시**: `"오늘 #인공지능연구회 채널에 올라온 메시지 스레드 정리해줘"`
- **동작**: Slack MCP를 통해 채널 목록, 스레드 읽기, 메시지 전송(승인 후) 수행

### ③ 먼데이 (Monday Admin) 연동
- **프롬프트 예시**: `"이번 주 진행 중인 프로젝트 보드 태스크 상태 확인해줘"`
- **동작**: Monday GraphQL API를 호출하여 보드 항목 및 담당자 현황 표시

### ④ 그룹웨어 (GW CLI/Skill) 연동
- **프롬프트 예시**: `"gw info 상태 확인하고, 결재 완료함에서 최근 3건 보여줘"`
- **동작**: `gw` CLI 스킬이 실행되어 본인 세션으로 결재함 읽기 및 지각일/출근 대조 수행

---

## 4부. 나만의 스킬(Skill) 직접 만들어보기 (17:45–17:55, 10분)

### `SKILL.md` 구조 파악하기
내 에이전트 폴더 (`~/.claude/skills/my-skill/SKILL.md` 또는 `~/.gemini/config/skills/my-skill/SKILL.md`)에 저장:

```markdown
---
name: daily-briefing
description: 매일 아침 메일, 슬랙 공지, 그룹웨어 결재 현황을 한눈에 브리핑하는 스킬
---

# 매일 아침 브리핑 스킬

이 스킬은 사용자가 "아침 브리핑 해줘"라고 할 때 작동합니다.

## 실행 순서
1. `gw approvals`를 실행하여 새로운 결재건 확인
2. 메일 스킬을 호출하여 읽지 않은 중요 메일 확인
3. 결과를 종합하여 오늘 할 일 요약 리프레시 작성
```

### 실습 미션
- 나만의 간단한 스킬 1개 만들기 (예: `hwp-to-pdf`, `weekly-report-draft`, `slack-auto-notice`)
- 에이전트에게 `"내가 방금 만든 스킬로 ~~ 해줘"` 실행해보기!

---

## 5부. Q&A 및 안전한 연동 수칙 (17:55–18:00, 5분)

1. **자격증명 보안 필수**: 토큰과 비밀번호는 절대 코드에 하드코딩하지 말고 `.env` 전역 관리
2. **승인형 모드 사용**: 삭제, 상신, 메시지 전송 등 변경 작업은 에이전트의 확인(Prompt)을 거쳐 실행
3. **다음 스텝**: 팀 내 공용 스킬 저장소 구축 및 업무 프로세스 자동화 공유

---

## 트러블슈팅 가이드

| 증상 |원인 | 해결책 |
|---|---|---|
| `gw: command not found` | uv tool install 미설치 또는 PATH 미등록 | `uv tool install --editable ~/dev/ji-gw-ai` 실행 후 터미널 재실행 |
| Slack MCP 승인 오류 | claude.ai 토큰 인증 만료 | claude.ai > Connectors > Slack 재인증 |
| 먼데이 API 에러 | API key 미설정 | `.env` 파일의 `MONDAY_API_TOKEN` 확인 |
| 스킬 인식이 안 됨 | 경로 위치 오류 | `~/.claude/skills/` 또는 `~/.gemini/config/skills/` 디렉토리에 링크되었는지 확인 |
