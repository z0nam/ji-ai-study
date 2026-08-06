# 60분 진행안 — MCP 연결과 Skill 만들기

- 일시: 2026-08-06 17:00~18:00
- 장소: 노꼬메
- 준비물: 개인 노트북, 사용할 서비스 계정, GitHub 접근 권한
- 완료 기준: **연결 1개 + 읽기 테스트 1회 + Skill 1개**

## 타임라인

| 시간 | 진행 | 참가자 산출물 |
|---|---|---|
| 17:00 | 성공 기준, MCP와 Skill의 차이 | 실습 목표 확인 |
| 17:08 | GitHub 자산 4종 소개, 경로 선택 | Slack·monday·Mail·GW 중 1개 선택 |
| 17:13 | 저장소 접근, 인증 정보와 안전 수칙 확인 | 필요한 저장소 확보 |
| 17:20 | 선택한 서비스 연결 | 로컬 CLI 또는 OAuth 연결 |
| 17:38 | 읽기 전용 스모크 테스트 | 실제 데이터가 포함된 결과 1개 |
| 17:48 | `SKILL.md` 작성·호출 | 재사용 가능한 Skill 폴더 1개 |
| 17:58 | Exit ticket | 연결·Skill·다음 업무 기록 |

## 1. 저장소 준비

Private 저장소는 Collaborator 초대 수락이 먼저입니다. 2분 안에 접근이 해결되지 않으면 공개 `mail-skill`로 전환합니다.

```bash
mkdir -p ~/dev
cd ~/dev

git clone https://github.com/z0nam/ji-gw-ai.git
git clone https://github.com/z0nam/ji-slack-admin.git
git clone https://github.com/z0nam/ji-monday-admin.git
git clone https://github.com/z0nam/mail-skill.git
```

이미 저장소가 있다면 다시 복제하지 말고 해당 폴더에서 `git status`와 `README.md`를 확인합니다.

## 2. 연결 경로 선택

### Slack 또는 monday — OAuth 경로

1. 사용 중인 AI 도구의 Connectors/MCP 메뉴에서 서비스를 선택합니다.
2. 제주연구원 계정으로 로그인하고 요청 권한을 확인합니다.
3. 아래처럼 쓰기 금지를 명시해 테스트합니다.

- Slack: `내가 볼 수 있는 Slack 채널 5개만 보여줘. 쓰지는 마.`
- monday: `내 monday 계정 정보와 최근 보드 3개를 읽기만 해줘.`

연결 메뉴가 없거나 관리자 정책에 막히면 GW 또는 Mail 경로로 이동합니다.

### 그룹웨어 — 로컬 CLI와 Skill

Python 3.12+와 `uv`가 필요합니다.

```bash
cd ~/dev/ji-gw-ai
uv sync
uv tool install --editable .
cp .env.example .env

mkdir -p ~/.claude/skills ~/.agents/skills
ln -s "$PWD/.claude/skills/gw" ~/.claude/skills/gw
ln -s "$PWD/.claude/skills/gw" ~/.agents/skills/gw

gw --help
gw info
```

실습에서는 `draft`, `submit`, `reserve`처럼 상태를 바꾸는 명령을 실행하지 않습니다.

### Mail — 공개 fallback 경로

Claude Code에서는 플러그인 설치가 가장 짧습니다.

```text
/plugin marketplace add z0nam/mail-skill
/plugin install mail-skill@mail-skill
/mail-skill:mailskill-setup
```

Codex에서는 저장소를 복제한 뒤 `plugins/mail-skill/skills/mail`을 사용자 Skill 폴더 `~/.agents/skills/mail`에 연결하고, 저장소 `README.md`의 bootstrap과 계정 설정을 따릅니다. 앱 비밀번호와 토큰은 로컬 키 저장소에만 둡니다.

## 3. 공통 읽기 테스트

본인이 연결한 서비스의 문장 하나를 그대로 실행합니다.

- Slack: `오늘 #인공지능연구회소모임의 새 글을 3줄로 요약해줘. 보내지는 마.`
- monday: `내가 접근 가능한 최근 보드 3개의 이름과 진행 중 항목 수를 읽어줘.`
- Mail: `최근 메일 5개의 제목·보낸 사람·받은 날짜만 표로 보여줘.`
- GW: `gw info를 확인하고, 가능한 read-only 명령 3개만 알려줘.`

서비스명·계정 또는 실제 항목이 반환되고 쓰기 동작이 없으면 성공입니다.

## 4. 오늘 브리핑 Skill 만들기

`today-brief/SKILL.md`에 다음을 명확히 적습니다.

1. 언제 호출할지: 예) `오늘 브리핑`, `업무 시작 정리`
2. 무엇을 읽을지: 오늘 일정과 연결한 서비스의 최근 항목
3. 어떻게 끝낼지: 중요도순 5개, 링크 포함, 발송·수정 금지

Codex에서는 `$skill-creator`를 쓰거나 저장소 범위 `.agents/skills`, 사용자 범위 `~/.agents/skills`에 직접 만들 수 있습니다. Claude Code에서는 `~/.claude/skills`를 사용합니다.

## 안전 수칙

- 중요한 보고서 폴더가 아닌 빈 작업 폴더에서 시작합니다.
- `.env`, OAuth 토큰, 앱 비밀번호, age 키를 채팅·화면 공유·Git 커밋에 노출하지 않습니다.
- 조회 성공을 먼저 확인하고 발송·상신·수정은 별도 명시 승인 후에만 합니다.

## Exit ticket

- 연결해 실제 결과를 확인한 서비스 1개
- 만든 `SKILL.md` 폴더 1개
- 다음 업무에서 적용할 반복 작업 1개
