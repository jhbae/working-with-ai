# Claude Code 완전 가이드

> Anthropic의 공식 CLI 에이전트. 터미널에서 동작하며 대규모 코드베이스 작업에 특화.

---

## 목차
1. [기본 설정](#1-기본-설정)
2. [CLAUDE.md 작성법](#2-claudemd-작성법)
3. [MCP (Model Context Protocol)](#3-mcp-model-context-protocol)
4. [Hooks (훅)](#4-hooks-훅)
5. [Skills & Commands](#5-skills--commands)
6. [플러그인 & 확장](#6-플러그인--확장)
7. [실용적 팁](#7-실용적-팁)
8. [트러블슈팅](#8-트러블슈팅)

---

## 1. 기본 설정

### 설치
```bash
# npm
npm install -g @anthropic-ai/claude-code

# 또는 직접 실행
npx @anthropic-ai/claude-code
```

### 초기화
```bash
cd your-project
claude
> /init    # CLAUDE.md 자동 생성
```

### 주요 슬래시 명령어
| 명령어 | 설명 |
|--------|------|
| `/init` | CLAUDE.md 초기화 |
| `/memory` | 메모리 파일 편집 |
| `/compact` | 컨텍스트 압축 |
| `/clear` | 대화 초기화 |
| `/help` | 도움말 |
| `#` | CLAUDE.md에 메모 추가 |

---

## 2. CLAUDE.md 작성법

### 파일 위치와 우선순위
```
~/.claude/CLAUDE.md        → 전역 (개인, 모든 프로젝트)
/project/CLAUDE.md         → 프로젝트 팀 (git 커밋 권장)
/project/.claude/CLAUDE.md → 프로젝트 개인 (gitignore)

우선순위: 프로젝트 개인 > 프로젝트 팀 > 전역
```

### 좋은 CLAUDE.md 예시
```markdown
# Project: My App

## Commands
- `npm run dev` - 개발 서버
- `npm run test` - 테스트 실행
- `npm run build` - 프로덕션 빌드

## Code Style
- TypeScript strict mode
- ES 모듈 사용 (CommonJS 금지)
- 컴포넌트: src/components/
- 유틸리티: src/utils/

## IMPORTANT
- .env 파일 절대 커밋 금지
- main 브랜치 직접 수정 금지
```

### 작성 원칙
| 원칙 | 설명 |
|------|------|
| **간결하게** | 컨텍스트는 제한된 자원. 필수 정보만 |
| **반복 개선** | 프롬프트처럼 튜닝. 효과 없으면 수정 |
| **IMPORTANT** | 중요한 규칙에 강조 표시 |
| **# 키 활용** | 코딩 중 바로 메모 추가 |

### 나쁜 예시 (피할 것)
```markdown
# 너무 많은 정보
- 모든 npm 스크립트 50개 나열
- 모든 폴더 구조 상세 설명
- 일반적인 코딩 규칙 반복
→ Claude가 관련 없는 정보 무시할 가능성 높음
```

---

## 3. MCP (Model Context Protocol)

### 개요
MCP는 Claude가 외부 도구/서비스와 통신하는 프로토콜.

### 권장 규칙
- 20-30개 MCP 설정 가능
- **동시 활성화는 10개 이하**
- 80개 이하 도구 유지

### 설정 방법
```bash
# MCP 서버 추가
claude mcp add filesystem -s user -- npx -y @modelcontextprotocol/server-filesystem ~/Documents

# 목록 확인
claude mcp list

# 상세 정보
claude mcp get github

# 제거
claude mcp remove github
```

### 인기 MCP 서버

| 서버 | 용도 | 설치 |
|------|------|------|
| **filesystem** | 로컬 파일 접근 | `@modelcontextprotocol/server-filesystem` |
| **[GitHub MCP](https://github.com/github/github-mcp-server)** | PR, 이슈, CI/CD | GitHub 공식 |
| **[Desktop Commander](https://github.com/wonderwhy-er/DesktopCommanderMCP)** | 터미널 + 파일 편집 | 커뮤니티 |
| **Sequential Thinking** | 구조화된 문제 해결 | 커뮤니티 |
| **postgres/mysql** | DB 쿼리 | 각 DB별 |

### 설정 파일 예시
```json
// ~/.claude/mcp.json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "~/projects"]
    },
    "github": {
      "command": "gh",
      "args": ["mcp"]
    }
  }
}
```

### Docker MCP Toolkit
220+ MCP 서버 카탈로그 제공: [Docker MCP Toolkit](https://www.docker.com/blog/add-mcp-servers-to-claude-code-with-mcp-toolkit/)

---

## 4. Hooks (훅)

### 개요
Hooks는 Claude 동작 시점에 실행되는 사용자 정의 스크립트.

### 훅 종류
| 훅 | 시점 | 용도 |
|----|------|------|
| **PreToolUse** | 도구 실행 전 | 검증, 차단 |
| **PostToolUse** | 도구 실행 후 | 포맷팅, 테스트 |
| **Notification** | 알림 발생 시 | 로깅, 알림 |
| **Stop** | 작업 완료 시 | 정리, 보고 |

### Exit 코드
- `0`: 허용/성공
- `2`: 차단 (PreToolUse에서만, stderr 메시지 Claude에 전달)
- 기타: 비차단 에러 (사용자에게 표시)

### 설정 파일
```json
// .claude/settings.json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [{
          "type": "command",
          "command": "[ \"$(git branch --show-current)\" != \"main\" ] || { echo '{\"block\": true, \"message\": \"main 브랜치 수정 금지\"}' >&2; exit 2; }"
        }]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [{
          "type": "command",
          "command": "npx prettier --write \"$CLAUDE_FILE_PATH\""
        }]
      }
    ]
  }
}
```

### 실용적인 훅 예시

#### 1. main 브랜치 보호
```json
{
  "matcher": "Edit|Write",
  "hooks": [{
    "type": "command",
    "command": "[ \"$(git branch --show-current)\" != \"main\" ] || exit 2"
  }]
}
```

#### 2. 자동 포맷팅 (TypeScript)
```json
{
  "matcher": "Edit|Write",
  "hooks": [{
    "type": "command",
    "command": "npx prettier --write \"$CLAUDE_FILE_PATH\""
  }]
}
```

#### 3. 민감 파일 차단
```json
{
  "matcher": "Read|Edit|Write",
  "hooks": [{
    "type": "command",
    "command": "echo \"$CLAUDE_FILE_PATH\" | grep -qE '\\.(env|pem|key)$' && exit 2 || exit 0"
  }]
}
```

#### 4. 테스트 자동 실행
```json
{
  "matcher": "Edit|Write",
  "hooks": [{
    "type": "command",
    "command": "npm run test:changed"
  }]
}
```

### 훅 관련 레포지토리
| 레포 | 설명 |
|------|------|
| [claude-code-hooks-mastery](https://github.com/disler/claude-code-hooks-mastery) | 8개 훅 이벤트 데모 |
| [claude-hooks](https://github.com/johnlindquist/claude-hooks) | TypeScript 기반, 타입 안전 |
| [claudekit](https://github.com/carlrannaberg/claudekit) | 명령어, 훅, 유틸리티 통합 |

---

## 5. Skills & Commands

### Skills vs Commands
| 구분 | Skills | Commands (슬래시) |
|------|--------|-------------------|
| 호출 | Claude가 자동 판단 | 사용자가 명시적 입력 |
| 용도 | 컨텍스트 기반 자동화 | 특정 작업 트리거 |

### Skills 설정
```
~/.claude/skills/
├── security-review.md
├── code-review.md
└── test-generator.md
```

### Skill 파일 예시
```markdown
<!-- ~/.claude/skills/security-review.md -->
# Security Review Skill

## Trigger
코드 리뷰, 보안 검토 요청 시

## Instructions
1. OWASP Top 10 체크
2. 인증/인가 로직 검토
3. 입력 검증 확인
4. 민감 정보 노출 체크

## Output Format
- 심각도: Critical/High/Medium/Low
- 위치: 파일:라인
- 설명: 문제점
- 해결책: 권장 수정
```

### 인기 Skills/Commands 레포
| 레포 | Stars | 설명 |
|------|-------|------|
| [Trail of Bits Security Skills](https://github.com/trailofbits) | - | CodeQL/Semgrep 보안 분석 |
| [Claude Command Suite](https://github.com/qdhenry/Claude-Command-Suite) | 900+ | 148+ 명령어, 54 에이전트 |
| [wshobson/commands](https://github.com/wshobson/commands) | 1.7K | 프로덕션 슬래시 명령어 |
| [SkillsMP](https://skillsmp.com) | - | 71K+ 스킬 마켓플레이스 |

---

## 6. 플러그인 & 확장

### 메모리/컨텍스트

#### claude-mem
세션 간 컨텍스트 유지. ChromaDB 벡터 저장.

```bash
# 설치
npm install -g claude-mem

# 사용
claude-mem init
```

**특징:**
- 자동 대화 압축
- 시작 시 관련 컨텍스트 로드
- 시맨틱 검색

**링크:** [github.com/thedotmack/claude-mem](https://github.com/thedotmack/claude-mem)

### 오케스트레이터

#### SuperClaude (19K+ stars)
16개 페르소나, 30개 명령어.

```bash
# 설치
pipx install superclaude
superclaude install
```

**주요 페르소나:**
| 페르소나 | 용도 |
|----------|------|
| architect | 시스템 설계 |
| frontend | UI/UX |
| backend | API, 인프라 |
| security | 보안 리뷰 |
| analyzer | 디버깅 |

**사용 예:**
```
/analyze --security
/build --frontend
/review --architect
```

**링크:** [github.com/SuperClaude-Org/SuperClaude_Framework](https://github.com/SuperClaude-Org/SuperClaude_Framework)

#### oh-my-claude-sisyphus
멀티 에이전트 오케스트레이션. 작업 완료까지 자동 반복.

```bash
npm install -g oh-my-claude-sisyphus
```

**특징:**
- 19개 에이전트
- 지능형 모델 라우팅 (Opus→Sonnet→Haiku)
- 18개 훅

**링크:** [github.com/Yeachan-Heo/oh-my-claude-sisyphus](https://github.com/Yeachan-Heo/oh-my-claude-sisyphus)

### IDE 통합

| 도구 | IDE | Stars |
|------|-----|-------|
| [claudecode.nvim](https://github.com/greggh/claude-code.nvim) | Neovim | 1.7K |
| [claude-code-chat](https://github.com/) | VS Code | 968 |
| [claude-code-ide.el](https://github.com/) | Emacs | 1.2K |

### 모니터링

| 도구 | 용도 |
|------|------|
| **CCFlare** | 웹 UI 대시보드, 토큰 소비 추적 |
| **CC Usage** | 로컬 로그 기반 분석 |

---

## 7. 실용적 팁

### 효율적인 프롬프트

#### ultrathink / think hard
깊은 사고가 필요할 때:
```
ultrathink: 이 아키텍처의 문제점을 분석해줘
```

#### 컨텍스트 제한 시
```
/compact    # 컨텍스트 압축
```

### 병렬 작업

#### Git Worktree 활용
```bash
# 기능별 worktree 생성
git worktree add ../feature-auth feature/auth
git worktree add ../feature-api feature/api

# 각각 독립적으로 Claude 실행
cd ../feature-auth && claude
cd ../feature-api && claude
```

### 비용 최적화

| 작업 유형 | 권장 모델 |
|-----------|-----------|
| 간단한 수정 | Haiku |
| 일반 개발 | Sonnet |
| 복잡한 설계 | Opus |

### 자주 쓰는 패턴

#### 1. 코드 리뷰 요청
```
이 PR의 변경사항을 리뷰해줘. 보안, 성능, 가독성 관점에서.
```

#### 2. 리팩토링
```
이 파일을 리팩토링해줘. 단, 기존 테스트가 통과해야 해.
```

#### 3. 디버깅
```
이 에러를 분석해줘: [에러 메시지]
관련 파일: src/api/auth.ts
```

#### 4. 테스트 작성
```
src/utils/validator.ts에 대한 단위 테스트를 작성해줘.
엣지 케이스 포함.
```

---

## 8. 트러블슈팅

### 자주 발생하는 문제

#### 컨텍스트 초과
```
증상: "Context limit exceeded" 에러
해결: /compact 실행 또는 새 세션 시작
```

#### MCP 연결 실패
```bash
# MCP 서버 상태 확인
claude mcp list

# 재설정
claude mcp remove [name]
claude mcp add [name] ...
```

#### 훅 실행 안됨
```
확인사항:
1. .claude/settings.json 문법 확인
2. 스크립트 실행 권한 (chmod +x)
3. 경로가 절대 경로인지 확인
```

#### Claude가 지시 무시
```
해결:
1. CLAUDE.md에 IMPORTANT 추가
2. 지시사항 간결하게 정리
3. 관련 없는 내용 제거
```

---

## 참고 자료

### 공식
- [Claude Code Docs](https://code.claude.com/docs)
- [Anthropic Best Practices](https://www.anthropic.com/engineering/claude-code-best-practices)

### 가이드
- [Writing a good CLAUDE.md](https://www.humanlayer.dev/blog/writing-a-good-claude-md)
- [Complete Guide to CLAUDE.md](https://www.builder.io/blog/claude-md-guide)

### 커뮤니티
- [awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code) (21K+ stars)
- [awesome-claude-plugins](https://github.com/quemsah/awesome-claude-plugins)

---

*마지막 업데이트: 2026-01-21*
