---
description: .gitignore 생성 → origin 연결 → prefix 규칙에 맞춘 커밋 → push까지 한 번에 처리
argument-hint: [커밋 메시지]
---

# /commit — 커밋 자동화 프롬프트

## 개요

| 항목 | 내용 |
| --- | --- |
| 목적 | 반복되는 초기 Git 작업(.gitignore, remote 연결)과 커밋·push를 한 번에 처리 |
| 입력값 | `$ARGUMENTS` — 커밋 메시지 본문 (prefix는 Claude가 판단해 붙임) |
| 원격 저장소 | `git@github.com:chaejinseong/a_commands.git` (remote 이름: `origin`) |
| 사용 예 | `/commit 로그인 API 추가` → `feat: 로그인 API 추가` |

---

## 실행 순서

아래 순서대로 작업해 줘.

### 1. `.gitignore` 생성

- **이미 `.gitignore` 파일이 있으면 이 단계는 생략한다.**
- 없으면 아래 항목을 모두 포함해 새로 만든다.
  - **에디터**: VS Code (`.vscode/`), JetBrains (`.idea/`)
  - **OS**: macOS (`.DS_Store`, `._*`), Windows (`Thumbs.db`, `Desktop.ini`)
  - **환경변수 파일**: `.env`, `.env.*` (단, `.env.example`은 제외하지 않음)
  - **기타 일반 항목**: 로그(`*.log`), 의존성 폴더(`node_modules/`, `venv/`, `.venv/`), 빌드 산출물(`dist/`, `build/`), 캐시(`__pycache__/`)

### 2. remote 연결

- **`origin`이 이미 연결되어 있으면 이 단계는 생략한다.**
- 연결되어 있지 않으면 아래 주소를 `origin`으로 추가한다.

```bash
git remote add origin git@github.com:chaejinseong/a_commands.git
```

### 3. 커밋 메시지 생성 및 커밋

- 변경 내용을 보고 아래 **prefix 규칙** 중 가장 알맞은 것을 골라 붙인다.
- 형식: `<prefix>: <커밋 메시지>`
- 커밋 메시지 본문: `$ARGUMENTS`

### 4. push

- `origin`으로 push한다.

---

## 커밋 prefix 규칙

`★` 표시는 자주 쓰는 prefix.

| prefix | 용도 |
| --- | --- |
| ★ `feat` | 새로운 기능 추가 |
| ★ `fix` | 버그 수정 |
| `docs` | 문서 수정 (README 등) |
| `style` | 코드 포맷 변경 (세미콜론, 공백 등) |
| ★ `refactor` | 리팩토링 (기능 변화 없음) |
| `test` | 테스트 코드 추가/수정 |
| `chore` | 빌드, 설정, 패키지, 기타 작업 |
| `perf` | 성능 개선 |
| `ci` | CI 설정 수정 |
| `build` | 빌드 시스템 수정 |
| ★ `revert` | 커밋 되돌리기 |

---

## 생략 조건 요약

| 조건 | 생략되는 단계 |
| --- | --- |
| `.gitignore`가 이미 있음 | 1단계 |
| `origin`이 이미 연결됨 | 2단계 |