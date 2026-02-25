---
name: commit
description: 변경사항을 분석하여 커밋 템플릿 형식으로 commit
disable-model-invocation: true
argument-hint: "[files...]"
---

You are helping create a git commit using the commit message template.

# Task Overview
변경사항을 분석하고 커밋 메시지 템플릿 형식에 맞는 메시지를 생성하여 commit합니다.

# Step 0. 커밋 템플릿 로드
다음 순서로 템플릿을 탐색하여 **처음 발견된 것**을 사용합니다.

1. **프로젝트 git 설정 확인**: `git config --local commit.template`
2. **프로젝트 루트의 템플릿 파일 탐색**: `.gitmessage`, `.git/commit_template`, `commit_template` 순서로 확인
3. **전역 git 설정 확인**: `git config --global commit.template`
4. **전역 기본 파일**: `~/.gitmessage`
5. **모두 없을 경우**: 아래 기본 템플릿을 사용

```
<타입>: <제목>

<본문> (선택)

<꼬리말> (선택)
```

탐색한 템플릿 파일 경로와 내용을 읽어 형식과 규칙을 파악한 뒤 이후 단계에서 적용합니다.

# Steps to Follow

## 1. 변경사항 확인
- `git status` 로 staged/unstaged 파일 목록 파악
- `git diff HEAD` 로 변경 내용 상세 분석

## 2. 스테이징 처리
- `$ARGUMENTS`가 있으면 해당 파일만 stage: `git add $ARGUMENTS`
- `$ARGUMENTS`가 없으면 전체 stage: `git add -A`
- stage 후 `git diff --staged` 로 최종 확인

## 3. 커밋 메시지 생성
변경사항을 분석하여 타입을 선택하고 메시지를 작성합니다.

**타입 선택 기준:**
- 새 파일/기능 추가 → `feat`
- 버그 수정 → `fix`
- 리팩토링 (동작 변경 없음) → `refactor`
- 주석/마크다운만 변경 → `docs`
- 스타일/포맷만 변경 → `style`
- 테스트 파일만 변경 → `test`
- 설정/빌드 파일 변경 → `chore`

**본문 작성 기준:**
- 변경 파일이 3개 이상이거나 변경 내용이 복잡한 경우 본문 추가
- "무엇을 변경했는지", "왜 변경했는지" 설명
- 하이픈(`-`)을 사용한 불렛 형식으로 작성

## 4. 생성된 메시지 확인 후 커밋 실행
생성된 커밋 메시지를 사용자에게 보여주고 진행합니다.
```bash
git commit -m "$(cat <<'EOF'
<생성된 커밋 메시지>
EOF
)"
```

## 5. 결과 확인
`git log --oneline -1` 로 커밋 결과를 사용자에게 표시합니다.

# Important Notes
- 커밋할 변경사항이 없으면 사용자에게 알림
- 에러 발생 시 원인을 설명하고 해결 방법 제시
- 커밋 메시지에 `Co-Authored-By` 문구를 포함하지 않는다
