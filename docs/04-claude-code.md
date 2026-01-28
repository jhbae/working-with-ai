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

> **2026 업데이트**: Anthropic이 MCP를 Linux Foundation의 **Agentic AI Foundation**에 기여. 오픈 거버넌스로 전환되어 에이전트 간 상호운용성 표준화 진행 중.

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

### 인기 MCP 서버 (카테고리별)

#### 공식 & 필수
| 서버 | 용도 | 설치 |
|------|------|------|
| **filesystem** | 로컬 파일 접근 | `@modelcontextprotocol/server-filesystem` |
| **[GitHub MCP](https://github.com/github/github-mcp-server)** | PR, 이슈, CI/CD | GitHub 공식 (3.2K stars) |
| **memory** | 지식 그래프 기반 메모리 | `@modelcontextprotocol/server-memory` |
| **fetch** | 웹 콘텐츠 가져오기 | 공식 |

#### 디자인 & UI
| 서버 | 용도 | 설치 |
|------|------|------|
| **[Figma MCP](https://mcp.figma.com)** | 디자인→코드 변환 | `claude mcp add --transport http figma https://mcp.figma.com/mcp` |
| **[claude-talk-to-figma](https://github.com/arinspunk/claude-talk-to-figma-mcp)** | 양방향 Figma 제어 | 커뮤니티 |

#### 브라우저 자동화
| 서버 | 용도 | 설치 |
|------|------|------|
| **[Playwright MCP](https://github.com/microsoft/playwright-mcp)** | 브라우저 자동화 (Microsoft 공식) | `@playwright/mcp` |
| **[Browserbase](https://github.com/browserbase/mcp-server-browserbase)** | 클라우드 브라우저 + Stagehand | `@browserbasehq/mcp` |
| **Puppeteer** | Chromium 자동화 | 커뮤니티 |

#### 데이터베이스
| 서버 | 용도 | 설치 |
|------|------|------|
| **[Supabase MCP](https://github.com/supabase-community/supabase-mcp)** | Supabase/Postgres RLS 인식 | `@supabase/mcp` |
| **PostgreSQL** | 직접 DB 쿼리 | 여러 구현 |
| **SQLite** | 로컬 DB 분석 | 공식 |

#### 검색 & AI
| 서버 | 용도 | 설치 |
|------|------|------|
| **[Perplexity MCP](https://github.com/perplexityai/modelcontextprotocol)** | 실시간 웹 검색 + AI | 공식 |
| **[Brave Search](https://github.com/anthropics/brave-search-mcp)** | 프라이버시 중심 검색 | 공식 |
| **[Exa](https://exa.ai)** | 시맨틱 검색 | 커뮤니티 |
| **[Context7](https://github.com/)** | 라이브러리 문서 검색 | 커뮤니티 |

#### 프로젝트 관리
| 서버 | 용도 | 설치 |
|------|------|------|
| **[Linear MCP](https://linear.app)** | 이슈 트래킹, 프로젝트 관리 | 공식 |
| **[Jira/Confluence](https://github.com/atlassian)** | Atlassian 통합 | Atlassian |
| **[Notion MCP](https://github.com/)** | Notion 워크스페이스 검색 | 커뮤니티 |

#### 커뮤니케이션
| 서버 | 용도 | 설치 |
|------|------|------|
| **[Slack MCP](https://github.com/)** | 채널 읽기, 메시지 전송 | 커뮤니티 |

#### 모니터링 & DevOps
| 서버 | 용도 | 설치 |
|------|------|------|
| **[Sentry MCP](https://github.com/)** | 에러 분석, 스택트레이스 | CLI 래퍼 |
| **[Desktop Commander](https://github.com/wonderwhy-er/DesktopCommanderMCP)** | 터미널 + 파일 편집 | 커뮤니티 |

#### 통합 서버
| 서버 | 용도 | 설치 |
|------|------|------|
| **[Rube](https://github.com/)** | 500+ 앱 연결 (Gmail, Slack, GitHub...) | 커뮤니티 |
| **[Pipedream](https://pipedream.com)** | 2,500 API + 8,000 도구 | 상용 |
| **[mcp-omnisearch](https://github.com/spences10/mcp-omnisearch)** | Tavily, Brave, Perplexity 통합 | 커뮤니티 |

### 설정 예시

#### Figma MCP 추가
```bash
claude mcp add --transport http figma https://mcp.figma.com/mcp
```

#### 설정 파일 예시
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
    },
    "supabase": {
      "command": "npx",
      "args": ["-y", "@supabase/mcp"],
      "env": {
        "SUPABASE_URL": "your-url",
        "SUPABASE_KEY": "your-key"
      }
    },
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp"]
    }
  }
}
```

### MCP 디렉토리
| 리소스 | URL | 설명 |
|--------|-----|------|
| **mcp.so** | [mcp.so](https://mcp.so) | 3,000+ 서버 인덱스 |
| **mcp-awesome.com** | [mcp-awesome.com](https://mcp-awesome.com) | 1,200+ 검증된 서버 |
| **Docker MCP Toolkit** | [링크](https://www.docker.com/blog/add-mcp-servers-to-claude-code-with-mcp-toolkit/) | 220+ 서버 카탈로그 |
| **공식 서버** | [github.com/modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers) | Anthropic 공식 |

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

> **플러그인 레지스트리**: [claude-plugins.dev](https://claude-plugins.dev) - 11,989개 플러그인, 63,065개 스킬

### 6.1 공식 & 핵심 (10K+ Stars)

| 플러그인 | Stars | 설명 |
|----------|-------|------|
| **[skills](https://github.com/anthropics/skills)** | 37.5K | 공식 에이전트 스킬 저장소 |
| **[agents](https://github.com/wshobson/agents)** | 25K | 프로덕션 서브에이전트 모음 |
| **[SuperClaude](https://github.com/SuperClaude-Org/SuperClaude_Framework)** | 20K | 9 페르소나, 19 명령어 (설정 프레임워크) |
| **[claudia](https://github.com/marcusbey/claudia)** | 19.9K | GUI 앱 & 에이전트 관리 도구 |
| **[vibe-kanban](https://github.com/)** | 14.7K | 칸반 보드 인터페이스 |
| **[claude-mem](https://github.com/thedotmack/claude-mem)** | 13.1K | 세션 간 메모리 유지 (ChromaDB) |
| **[Claude-Flow](https://github.com/ruvnet/claude-flow)** | 11.4K | 멀티 에이전트 스웜 오케스트레이션 |
| **[zen-mcp-server](https://github.com/)** | 10.8K | 다중 모델 통합 MCP |

### 6.2 오케스트레이션 & 확장 프레임워크

#### Claude-Flow (오케스트레이터)
멀티 에이전트 스웜 플랫폼. 54+ 전문 에이전트 배포.

```bash
# 설치
npm install -g claude-flow

# 실행
claude-flow init
```

**특징:**
- 분산 스웜 인텔리전스
- RAG 통합
- 공유 메모리 & 합의 시스템
- 500K+ 다운로드, 100K 월간 사용자

**링크:** [github.com/ruvnet/claude-flow](https://github.com/ruvnet/claude-flow)

---

#### Claude-Squad (오케스트레이터, 5.6K Stars)
여러 AI 에이전트 동시 관리 (Claude Code, Aider, Codex, OpenCode, Amp).

```bash
# 설치
brew install claude-squad

# 실행 (cs 명령어)
cs
```

**특징:**
- tmux 기반 격리된 터미널 세션
- git worktree로 브랜치 분리
- 백그라운드 작업 (yolo/auto-accept 모드)

**링크:** [github.com/smtg-ai/claude-squad](https://github.com/smtg-ai/claude-squad)

---

#### SuperClaude (설정 프레임워크, 20K Stars)
9개 페르소나, 19개 명령어로 단일 Claude 세션 향상. *병렬 실행/오케스트레이션 아님.*

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

#### oh-my-claude-sisyphus (오케스트레이터)
멀티 에이전트 오케스트레이션. 작업 완료까지 자동 반복.

```bash
npm install -g oh-my-claude-sisyphus
```

**특징:**
- 19개 에이전트
- 지능형 모델 라우팅 (Opus→Sonnet→Haiku)
- 18개 훅

**링크:** [github.com/Yeachan-Heo/oh-my-claude-sisyphus](https://github.com/Yeachan-Heo/oh-my-claude-sisyphus)

### 6.3 GUI 클라이언트

#### Claudia (19.9K Stars)
터미널이 익숙하지 않은 사용자를 위한 데스크톱 앱.

```bash
# macOS
brew install --cask claudia

# 또는 GitHub Releases에서 다운로드
```

**특징:**
- Tauri 2 기반 네이티브 앱
- 커스텀 에이전트 생성
- 세션 관리 & 사용량 추적
- 백그라운드 에이전트 실행

**링크:** [github.com/marcusbey/claudia](https://github.com/marcusbey/claudia)

#### 기타 GUI
| 도구 | Stars | 설명 |
|------|-------|------|
| **happy** | 7K | 모바일/웹 클라이언트 |
| **claudecodeui** | 5.4K | 데스크톱/모바일 UI |

### 6.4 사용량 모니터링

#### ccusage (9.7K Stars)
로컬 JSONL 로그 분석 CLI.

```bash
# 설치 불필요, npx로 바로 실행
npx ccusage daily     # 일별 사용량
npx ccusage monthly   # 월별 집계
npx ccusage session   # 세션별 분석
npx ccusage blocks    # 5시간 빌링 윈도우
```

**링크:** [github.com/ryoppippi/ccusage](https://github.com/ryoppippi/ccusage)

#### Claude-Code-Usage-Monitor (6.1K Stars)
실시간 터미널 모니터링 + ML 예측.

```bash
pip install claude-monitor
claude-monitor  # 또는 cmonitor, ccmonitor
```

**특징:**
- Rich UI 프로그레스 바
- 플랜 자동 감지 (Pro 44K, Max5 88K, Max20 220K)
- 다단계 경고 시스템

**링크:** [github.com/Maciek-roboblog/Claude-Code-Usage-Monitor](https://github.com/Maciek-roboblog/Claude-Code-Usage-Monitor)

#### 기타 모니터링
| 도구 | 플랫폼 | 설명 |
|------|--------|------|
| **ccusage-vscode** | VS Code | 상태바에 사용량 표시 |
| **ccusage Raycast** | Raycast | macOS 빠른 조회 |

### 6.5 개발 워크플로우

#### TDD Guard (1.7K Stars)
TDD 원칙 자동 강제.

```bash
# Claude Code에서 /hooks로 설정
# matcher: Write|Edit|MultiEdit|TodoWrite
# hook: tdd-guard
```

**차단 규칙:**
- 실패하는 테스트 없이 구현 금지
- 테스트 요구사항 초과 구현 금지
- 동시에 여러 테스트 추가 금지

#### Continuous-Claude-v2 (2.2K Stars)
컨텍스트 관리 & 에이전트 오케스트레이션.

**특징:**
- 훅으로 상태 유지 (ledger & handoff)
- MCP 실행 시 컨텍스트 오염 방지
- 격리된 컨텍스트 윈도우

#### cc-sessions (1.5K Stars)
개발 세션 추적 & git 관리.

```bash
npm install -g cc-sessions
```

### 6.6 IDE 통합

| 도구 | IDE | Stars | 특징 |
|------|-----|-------|------|
| **[claudecode.nvim](https://github.com/greggh/claude-code.nvim)** | Neovim | 1.7K | 완전 통합 |
| **[claude-code-ide.el](https://github.com/)** | Emacs | 1.2K | MCP 네이티브 |
| **[claude-code-chat](https://github.com/)** | VS Code | 968 | 채팅 인터페이스 |

### 6.7 유틸리티 & 기타

| 도구 | Stars | 설명 |
|------|-------|------|
| **claude-code-prompt-improver** | 1K | 프롬프트 자동 개선 훅 |
| **Claude-Code-Remote** | 943 | 이메일로 원격 제어 |
| **claude-code-hooks-multi-agent-observability** | 893 | 멀티 에이전트 실시간 모니터링 |
| **claude-code-proxy** | 2.8K | OpenAI 모델 지원 프록시 |
| **claude-code-templates** | 15.4K | 100+ 에이전트/명령어 템플릿 |

### 6.8 플러그인 설치 방법

```bash
# 마켓플레이스에서 설치
claude
> /plugin install [plugin-name]

# 또는 settings.json에 직접 추가
# .claude/settings.json
```

### 6.9 플러그인 디스커버리

| 리소스 | URL | 설명 |
|--------|-----|------|
| **공식 디렉토리** | `/plugin list` | Claude Code 내장 |
| **claude-plugins.dev** | [링크](https://claude-plugins.dev) | 11,989 플러그인 레지스트리 |
| **SkillsMP** | [skillsmp.com](https://skillsmp.com) | 71K+ 스킬 마켓플레이스 |
| **awesome-claude-code** | [GitHub](https://github.com/hesreallyhim/awesome-claude-code) | 21K+ 커뮤니티 큐레이션 |
| **ccplugins marketplace** | [GitHub](https://github.com/ccplugins/marketplace) | 243개 검증된 플러그인 |

---

## 7. 실용적 팁

### 7.1 Boris Cherny의 프로 팁 (Claude Code 창시자)

> 출처: [How Boris Uses Claude Code](https://howborisusesclaudecode.com/)
> "설정은 놀랍도록 기본적. Claude Code는 별다른 커스터마이징 없이도 잘 동작함."

#### 핵심 워크플로우

**1. 5개 병렬 세션 운영**
```bash
# 터미널 탭 1-5: 동일 저장소의 독립적인 git checkout
# 시스템 알림으로 입력 필요 시점 감지
# + claude.ai/code에서 5-10개 추가 세션
```

**2. Plan 모드로 시작 (shift+tab 2번)**
```
Plan 모드 → 계획 정제 → 자동 수락 모드 → 단일 패스 실행
```
> Claude가 대부분 1-shot으로 해결

**3. Opus 4.5 + Thinking 고정**
> "더 크고 느리지만, 지시를 덜 해도 되고 도구 사용이 좋아서 결국 더 빠름"

**4. 팀 공유 CLAUDE.md**
```markdown
# Git에 체크인, 팀 전체가 주간 업데이트
# Claude가 실수하면 → CLAUDE.md에 추가 → 반복 방지
```

**5. 슬래시 명령어로 자동화**
```bash
# .claude/commands/commit-push-pr.md
# → 하루 수십 번 실행

# 인라인 bash로 컨텍스트 사전 계산 (git status 등)
# → 모델과의 왕복 최소화
```

**6. PostToolUse 훅으로 자동 포맷**
```json
{
  "PostToolUse": [{
    "matcher": "Edit|Write",
    "hooks": [{"type": "command", "command": "bun run format"}]
  }]
}
```
> Claude 코드의 마지막 10% 포맷 문제 해결 → CI 실패 방지

**7. /permissions로 안전하게 권한 관리**
```
--dangerously-skip-permissions 대신 /permissions 사용
→ 팀 자산으로 공유, 리뷰, 버전 관리
```

#### 가장 중요한 팁: 검증 피드백 루프

> **"Claude에게 작업 검증 방법을 제공하면 최종 결과 품질이 2-3배 향상"**

| 도메인 | 검증 방법 |
|--------|-----------|
| 백엔드 | 테스트 스위트 실행 |
| 프론트엔드 | 브라우저에서 확인 |
| 모바일 | 시뮬레이터 테스트 |
| CLI | bash 명령어 실행 |

```markdown
# CLAUDE.md 예시
## 검증
- 모든 변경 후 `npm test` 실행
- UI 변경 시 `npm run dev`로 브라우저 확인
- API 변경 시 `curl` 테스트
```

---

### 7.2 효율적인 프롬프트

#### Thinking Mode (2026년 1월~)
> `ultrathink`, `think hard`, `think` 키워드는 **deprecated** (2026-01-16)
> 이제 thinking mode가 기본 활성화 (31,999 토큰)

```
TAB 키로 thinking mode on/off 토글
```

| 이전 (v1) | 현재 (v2+) |
|-----------|-----------|
| think, think hard, ultrathink | TAB으로 토글 |
| 키워드로 사고 깊이 조절 | 기본 max 토큰 |

#### 컨텍스트 제한 시
```
/compact    # 컨텍스트 압축
```

### 7.3 병렬 작업

#### Git Worktree 활용
```bash
# 기능별 worktree 생성
git worktree add ../feature-auth feature/auth
git worktree add ../feature-api feature/api

# 각각 독립적으로 Claude 실행
cd ../feature-auth && claude
cd ../feature-api && claude
```

#### Claude Squad로 멀티 에이전트
```bash
brew install claude-squad
cs  # 여러 에이전트 동시 관리
```

### 7.4 비용 최적화

| 작업 유형 | 권장 모델 | Boris의 선택 |
|-----------|-----------|--------------|
| 간단한 수정 | Haiku | - |
| 일반 개발 | Sonnet | - |
| 복잡한 설계 | Opus | **Opus 4.5 고정** |

> Boris: "Opus가 결국 더 빠름" (지시 횟수 감소)

### 7.5 자주 쓰는 패턴

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
- [How Boris Uses Claude Code](https://howborisusesclaudecode.com/) - 창시자의 워크플로우 ⭐
- [Writing a good CLAUDE.md](https://www.humanlayer.dev/blog/writing-a-good-claude-md)
- [Complete Guide to CLAUDE.md](https://www.builder.io/blog/claude-md-guide)

### 커뮤니티
- [awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code) (21K+ stars)
- [awesome-claude-plugins](https://github.com/quemsah/awesome-claude-plugins)

---

*마지막 업데이트: 2026-01-21*
