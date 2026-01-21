# AI 기반 개발 도구 & 방법론 종합 가이드 - 작성 계획

## 문서 목표
AI 코딩 도구들의 현황, 설정 방법, 개발 방법론을 **실용적 관점**에서 정리하여 개발자가 자신에게 맞는 도구와 워크플로우를 선택할 수 있도록 돕는다.

---

## 1. 개요 및 현황 (01-overview.md)

### 1.1 AI 코딩 도구 채택 현황
- **2025-2026 데이터 기반 현실적 그림**
  - 85% 개발자가 AI 도구 사용 (Stack Overflow 2025)
  - 하지만 52%는 에이전트 미사용 또는 간단한 도구만 사용
  - Agentic AI 정기 사용자는 25%에 불과
  - "Vibe coding"은 72%가 전문 업무에 사용 안함

### 1.2 신뢰와 우려
- 87% 정확도 우려, 81% 보안/프라이버시 우려
- 도구를 "가속기"로 사용 vs "위임자"로 사용하는 차이

### 1.3 도구 분류 체계
```
┌─────────────────────────────────────────────────────────────┐
│                    AI 코딩 도구 스펙트럼                      │
├─────────────────────────────────────────────────────────────┤
│  자동완성         IDE 통합          CLI 에이전트      오케스트레이터  │
│  ────────────────────────────────────────────────────────→  │
│  Copilot         Cursor            Claude Code     SuperClaude│
│  Tabnine         Windsurf          Gemini CLI      oh-my-*   │
│                  Cline             OpenCode        moai-adk  │
│                                    Aider           Kiro      │
│                                    Codex                     │
│  ← 낮은 자율성                          높은 자율성 →          │
└─────────────────────────────────────────────────────────────┘
```

---

## 2. 주요 도구 비교 (02-tools.md)

### 2.1 주류 도구 (높은 채택률)

| 도구 | 채택률 | 특징 | 가격 |
|------|--------|------|------|
| GitHub Copilot | 68% | 자동완성 중심, IDE 통합 | $10-19/월 |
| ChatGPT | 82% | 범용, 코드 외 작업도 | $20/월 |
| Claude Code | 45% (전문개발자) | 대규모 리팩토링, 200K 컨텍스트 | $20-200/월 |
| Cursor | 19.3% (agentic) | IDE 경험, 실시간 편집 | $20/월 |

### 2.2 CLI 에이전트 비교

#### Claude Code
- **강점**: 멀티파일 작업, 실제 200K 컨텍스트, 30% 적은 재작업
- **약점**: 비용 ($100-200 Max), 터미널 기반
- **적합**: 대규모 리팩토링, 복잡한 버그, 설계 수준 변경

#### OpenCode
- **강점**: 오픈소스, BYOK(모델 자유 선택), 병렬 세션, LSP 통합
- **약점**: 상대적으로 신생
- **적합**: 비용 민감, 프라이버시 중시, 온프레미스 필요

#### Gemini CLI
- **강점**: Google 생태계 통합, Conductor로 컨텍스트 기반 개발
- **약점**: Claude/GPT 대비 코딩 성능 논쟁
- **적합**: Google Cloud 사용자, 실험적 사용

#### Aider
- **강점**: Git 통합, 투명한 diff 기반 편집
- **약점**: 설정 복잡도
- **적합**: Git 워크플로우 중시, 명시적 제어 선호

### 2.3 오케스트레이터/프레임워크

#### SuperClaude (19k+ stars)
- 16개 인지 페르소나, 30개 명령어
- `/analyze --frontend`, `/review --security` 등
- **현실**: 커뮤니티 주도, Anthropic 공식 아님

#### oh-my-opencode / oh-my-claude-sisyphus
- 멀티 에이전트 오케스트레이션
- Sisyphus: "작업 완료까지 계속 굴리는" 에이전트
- 지능형 모델 라우팅 (Opus→Sonnet→Haiku)
- **현실**: 파워유저용, 학습 곡선 있음

#### moai-adk
- SPEC-First DDD 방법론
- `/moai:1-plan` → `/moai:2-run` → `/moai:3-sync` 워크플로우
- 20개 에이전트 + 49개 스킬
- **현실**: 한국어 지원, 구조화된 접근

---

## 3. Claude Code 설정 가이드

> **상세 내용은 [docs/claude-code.md](docs/claude-code.md) 참조**

### 핵심 요약
- **CLAUDE.md**: 프로젝트 설정 파일 (간결하게, 반복 개선)
- **MCP**: 20-30개 설정, 10개 이하 활성화
- **Hooks**: PreToolUse/PostToolUse로 워크플로우 자동화
- **Skills**: 모델이 자동 호출하는 전문 기능
- **플러그인**: claude-mem, SuperClaude, claudekit 등

---

## 4. AI 개발 방법론 (04-methodology.md)

### 4.1 현재 주요 방법론

#### Vibe Coding
- 프롬프트로 전체 앱 생성
- **현실**: 72%가 전문 업무에 미사용
- **적합**: 프로토타입, 학습, 해커톤

#### Spec-Driven Development (SDD)
- requirements.md → design.md → tasks.md
- 에이전트 행동의 "진실의 원천"
- **도구**: Kiro, GitHub Spec-Kit, cc-sdd, moai-adk

#### Context-Driven Development
- Gemini CLI의 Conductor
- 마크다운으로 스펙과 플랜 공식화

#### ANALYZE-PRESERVE-IMPROVE (moai-adk)
- 기존 동작 보존하며 개선
- 재작업 90% 감소 주장

### 4.2 실용적 접근법

```
┌────────────────────────────────────────────────────┐
│           작업 복잡도에 따른 방법론 선택             │
├────────────────────────────────────────────────────┤
│                                                    │
│  간단한 수정     중간 기능        복잡한 시스템      │
│  ──────────────────────────────────────────────→  │
│  직접 코딩       Plan Mode        Spec-Driven      │
│  자동완성        구조화된 프롬프트   문서 선행        │
│                                                    │
│  Copilot        Claude Code      Kiro/moai-adk   │
│  Cursor         /plan 명령어      requirements.md  │
└────────────────────────────────────────────────────┘
```

---

## 5. 병렬 작업 & 페르소나 (05-parallel.md)

### 5.1 병렬 작업 방법

#### Git Worktree 활용
```bash
# 메인에서 기능별 worktree 생성
git worktree add ../feature-auth feature/auth
git worktree add ../feature-api feature/api

# 각 worktree에서 독립적으로 Claude 실행
cd ../feature-auth && claude
cd ../feature-api && claude
```

#### 백그라운드 에이전트 (Claude Code)
```
> /parallel "테스트 작성" "문서 업데이트" "린팅 수정"
```

#### oh-my-opencode 병렬 실행
- Sisyphus가 자동으로 작업 분배
- 플래너, 구현자, 리뷰어 병렬 동작

### 5.2 페르소나 활용

#### SuperClaude 페르소나
| 페르소나 | 용도 | 사용법 |
|----------|------|--------|
| architect | 시스템 설계 | `/analyze --architect` |
| frontend | UI/UX, 접근성 | `/build --frontend` |
| backend | API, 인프라 | `/code --backend` |
| security | 보안 리뷰 | `/review --security` |
| analyzer | 디버깅, 분석 | `/debug --analyzer` |

#### 페르소나 효과
- 전문 영역에 맞는 관점 제공
- 일관된 코드 스타일 유지
- **주의**: 만능이 아님, 기본 Claude도 대부분 가능

### 5.3 멀티 에이전트 오케스트레이션

```
┌─────────────────────────────────────────────┐
│           oh-my-claude-sisyphus 구조         │
├─────────────────────────────────────────────┤
│                                             │
│    ┌──────────────────────┐                │
│    │  Sisyphus (Opus)     │ ← 오케스트레이터 │
│    │  복잡도 분석 & 라우팅  │                │
│    └──────────┬───────────┘                │
│               │                            │
│    ┌──────────┼──────────┐                │
│    ▼          ▼          ▼                │
│  Haiku     Sonnet      Opus              │
│  간단작업    중간작업    복잡작업           │
│                                             │
└─────────────────────────────────────────────┘
```

---

## 6. Spec-Driven 개발 도구 비교 (06-spec-driven.md)

### 6.1 도구별 비교

| 도구 | 플랫폼 | 특징 | 성숙도 |
|------|--------|------|--------|
| **Kiro** (AWS) | IDE | spec/vibe 모드 전환, DevOps 자동화 | 초기 (성능 이슈 보고) |
| **GitHub Spec-Kit** | CLI | 다양한 코딩 도구 지원, 슬래시 명령 | 초기 |
| **cc-sdd** | 설정파일 | Claude Code/Cursor/Gemini 등 지원 | 커뮤니티 |
| **OpenSpec** | 프레임워크 | 범용 SDD | 커뮤니티 |
| **moai-adk** | CLI | SPEC-First DDD, 한국어, 자동화 | 활발 |

### 6.2 SDD 워크플로우 예시 (moai-adk)

```bash
# 1. 프로젝트 분석
/moai:0-project

# 2. SPEC 생성
/moai:1-plan "사용자 인증 기능 추가"
# → SPEC-001.md 생성 (EARS 형식)

# 3. 구현
/moai:2-run SPEC-001

# 4. 검증 & 동기화
/moai:3-sync SPEC-001
```

### 6.3 SDD vs Vibe Coding

| 측면 | Spec-Driven | Vibe Coding |
|------|-------------|-------------|
| 속도 | 느림 (문서화 선행) | 빠름 |
| 품질 | 높음 (명시적 요구사항) | 가변적 |
| 재작업 | 적음 | 많음 |
| 적합 | 프로덕션, 팀 | 프로토타입, 개인 |

### 6.4 Google Antigravity
- 2025년 말 출시 (Windsurf 인수 기반)
- 자율 에이전트 중심 개발
- 아직 초기 단계, 관망 권장

---

## 7. 실용적 권장사항 (README.md에 요약)

### 7.1 시작하기
1. **입문**: Cursor 또는 GitHub Copilot로 시작
2. **성장**: Claude Code 추가 (복잡한 작업용)
3. **고급**: 필요시 SuperClaude/moai-adk 검토

### 7.2 현실적 기대치
- AI 도구는 **가속기**지 **대체재**가 아님
- 코드 리뷰는 여전히 필수
- 87%가 정확도 우려 → 검증 필수

### 7.3 비용 대비 효과
```
가성비 순서:
1. GitHub Copilot ($10-19) - 일상 자동완성
2. Cursor Pro ($20) - IDE 통합 에이전트
3. Claude Pro ($20) - 복잡한 작업 escalation
4. Claude Max ($100-200) - 대규모 프로젝트
```

---

---

## 8. 도구 종합 목록 (GitHub Stars 기준, 1K+ 위주)

### 8.1 공식/주류 도구 (S-Tier)

| 도구 | Stars | 분류 | 특징 |
|------|-------|------|------|
| **Claude Code** | 55K+ | CLI 에이전트 | Anthropic 공식, 200K 컨텍스트, 멀티파일 → [상세 가이드](docs/claude-code.md) |
| **[Amp Code](https://ampcode.com)** | - | CLI/IDE | Sourcegraph, 멀티모델(Opus/GPT-5.2), Claude Skills 호환 |
| **GitHub Copilot** | - | IDE 통합 | 68% 채택률, Agent Mode, MCP 지원 |
| **Cursor** | - | IDE | Copilot fork, 벡터 기반 이해, 체크포인트 |
| **Windsurf** | - | IDE | Codeium, Riptide 검색, $15/월 |

#### Amp Code vs Claude Code
| 측면 | Claude Code | Amp Code |
|------|-------------|----------|
| 모델 | Anthropic 위주 | 멀티모델 (유연함) |
| 팀 협업 | 기본 | 스레드/컨텍스트 공유 기본 |
| Skills | 원본 | Claude Skills 호환 |
| 모드 | 단일 | Smart/Rush (비용 최적화) |
| 가격 | $20-200/월 | 매일 $10 무료 + 유료 |

> "Claude Code를 이긴 첫 번째 도구" - Glen Maddern, Cloudflare

### 8.2 CLI 에이전트 (오픈소스)

| 도구 | Stars | 특징 |
|------|-------|------|
| **[OpenCode](https://github.com/opencode-ai/opencode)** | 활발 | BYOK, 병렬 세션, LSP 통합, 온프레미스 |
| **[Aider](https://github.com/paul-gauthier/aider)** | 25K+ | Git 통합, diff 기반 편집, 멀티 LLM |
| **[Plandex](https://github.com/plandex-ai/plandex)** | 활발 | 2M 토큰 컨텍스트, 30+ 언어 지원 |

### 8.3 IDE/에디터 통합

| 도구 | Stars | 특징 |
|------|-------|------|
| **[Cline](https://github.com/cline/cline)** | 40K+ | VS Code, Plan/Act 모드, 자율 에이전트 |
| **[Roo Code](https://github.com/RooVetGit/Roo-Cline)** | 10K+ | VS Code, 멀티 에이전트, 역할 기반 |
| **[Continue](https://github.com/continuedev/continue)** | 25K+ | VS Code/JetBrains, 엔터프라이즈, 셀프호스트 |
| **[Tabby](https://github.com/TabbyML/tabby)** | 25K+ | 셀프호스트 Copilot 대안, 오픈소스 |
| **[Cody](https://sourcegraph.com/cody)** | - | Sourcegraph, 대형 모노레포 특화 |
| **[Zed AI](https://zed.dev/ai)** | 55K+ | 고성능 에디터, ACP 프로토콜 |

### 8.4 Claude Code 생태계 (확장/플러그인)

| 도구 | Stars | 분류 | 용도 |
|------|-------|------|------|
| **[claude-code-sdk-python](https://github.com/anthropics/claude-code-sdk-python)** | 4K | SDK | 공식 Python SDK |
| **[claude-code-security-review](https://github.com/anthropics/claude-code-security-review)** | 2.8K | Action | 보안 리뷰 GitHub Action |
| **[claudecode.nvim](https://github.com/greggh/claude-code.nvim)** | 1.7K | IDE | Neovim 통합 |
| **[claude-code-chat](https://github.com/)** | 968 | IDE | VS Code 채팅 인터페이스 |
| **[claude-mem](https://github.com/thedotmack/claude-mem)** | 활발 | 메모리 | 세션 간 컨텍스트 유지, ChromaDB |
| **[claude-sessions](https://github.com/)** | 1.1K | 명령어 | 개발 세션 추적 |
| **[claude-canvas](https://github.com/)** | 1.1K | TUI | Claude Code용 디스플레이 툴킷 |

### 8.5 오케스트레이터/프레임워크

| 도구 | Stars | 특징 |
|------|-------|------|
| **[SuperClaude](https://github.com/SuperClaude-Org/SuperClaude_Framework)** | 19K+ | 16 페르소나, 30 명령어, pipx 설치 |
| **[oh-my-opencode](https://github.com/code-yeongyu/oh-my-opencode)** | 활발 | Sisyphus 에이전트, 25+ 훅, MCP 통합 |
| **[oh-my-claude-sisyphus](https://github.com/Yeachan-Heo/oh-my-claude-sisyphus)** | 활발 | Claude Code용 Sisyphus, 모델 라우팅 |
| **[moai-adk](https://github.com/modu-ai/moai-adk)** | 활발 | SPEC-First DDD, 한국어, 20 에이전트 |
| **[vibe-tools](https://github.com/eastlondoner/vibe-tools)** | 활발 | Cursor/Claude Code/Codex 지원, Gemini 2.5 |

### 8.6 오픈소스 AI 에이전트

| 도구 | Stars | 특징 |
|------|-------|------|
| **[SWE-agent](https://github.com/SWE-agent/SWE-agent)** | 15K+ | SWE-bench SOTA, NeurIPS 2024 |
| **[Devika/Opcode](https://github.com/stitionai/devika)** | 20K+ | Devin 대안, 웹 리서치 + 코딩 |
| **[GPT Engineer](https://github.com/gpt-engineer-org/gpt-engineer)** | 55K+ | 전체 코드베이스 생성 |
| **[Smol Developer](https://github.com/smol-ai/developer)** | 12K+ | 경량 프로토타입 에이전트 |
| **[MetaGPT](https://github.com/geekan/MetaGPT)** | 50K+ | 멀티 에이전트, PRD→코드 |

### 8.7 앱 빌더 (Vibe Coding)

| 도구 | 특징 | 적합 |
|------|------|------|
| **[Lovable](https://lovable.dev)** | $100M ARR, 가장 빠른 성장, Supabase 통합 | MVP, 사이드 프로젝트 |
| **[v0](https://v0.app)** (Vercel) | React/Next.js 특화, 에이전트 모드 | 프론트엔드 |
| **[Bolt.new](https://bolt.new)** | 브라우저 기반, WebContainer, 오픈소스 | 빠른 프로토타입 |
| **[Replit Agent](https://replit.com)** | 다국어, Plan/Build 모드, 풀스택 | 학습, 실험 |

### 8.8 Spec-Driven 도구

| 도구 | 플랫폼 | 특징 |
|------|--------|------|
| **Kiro** (AWS) | IDE | spec/vibe 모드, DevOps 자동화 (초기) |
| **[GitHub Spec-Kit](https://github.com/github/spec-kit)** | CLI | 다양한 코딩 도구 지원 |
| **[cc-sdd](https://github.com/gotalab/cc-sdd)** | 설정 | Claude Code/Cursor/Gemini 등 |
| **[OpenSpec](https://github.com/Fission-AI/OpenSpec)** | 프레임워크 | 범용 SDD |

### 8.9 Claude Code Hooks 레포지토리

| 도구 | 용도 |
|------|------|
| **[claude-code-hooks-mastery](https://github.com/disler/claude-code-hooks-mastery)** | 8개 훅 이벤트 데모 |
| **[claude-hooks](https://github.com/johnlindquist/claude-hooks)** | TypeScript 기반, 타입 안전 |
| **[claudekit](https://github.com/carlrannaberg/claudekit)** | 명령어, 훅, 유틸리티 통합 |
| **[claude-code-showcase](https://github.com/ChrisWiles/claude-code-showcase)** | 훅, 스킬, 에이전트 예제 |

### 8.10 AI 코딩 모델 (로컬/오픈소스)

| 모델 | 제공자 | 특징 |
|------|--------|------|
| **Qwen3-Coder** | Alibaba | 오픈소스 SOTA, 256K 컨텍스트, SWE-bench 69.6% |
| **DeepSeek Coder V2** | DeepSeek | 128K 컨텍스트, 338 언어, MoE 236B |
| **Codestral** | Mistral | 80+ 언어, HumanEval 86.6% |
| **StarCoder2** | BigCode | 오픈소스, 다양한 크기 |

### 8.11 MCP 서버 (인기)

| 서버 | 용도 |
|------|------|
| **filesystem** | 로컬 파일 시스템 접근 |
| **[GitHub MCP](https://github.com/github/github-mcp-server)** | PR, 이슈, CI/CD 통합 |
| **[Desktop Commander](https://github.com/wonderwhy-er/DesktopCommanderMCP)** | 터미널 + 파일 편집 |
| **Sequential Thinking** | 구조화된 문제 해결 |
| **[Docker MCP Toolkit](https://www.docker.com/blog/add-mcp-servers-to-claude-code-with-mcp-toolkit/)** | 220+ MCP 서버 카탈로그 |

### 8.12 큐레이션 목록 (Awesome Lists)

| 목록 | Stars | 설명 |
|------|-------|------|
| **[awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code)** | 21K+ | 스킬, 훅, 명령어 종합 |
| **[awesome-claude-plugins](https://github.com/quemsah/awesome-claude-plugins)** | 활발 | 243개 플러그인 인덱싱 |
| **[awesome-ai-agents](https://github.com/e2b-dev/awesome-ai-agents)** | 15K+ | AI 에이전트 종합 |
| **[awesome-ai-devtools](https://github.com/jamesmurdza/awesome-ai-devtools)** | 활발 | AI 개발 도구 전반 |

---

## 9. 기타 주목할 도구

### 9.1 기업/상용
- **Amazon Q Developer**: AWS 통합, 코드 리뷰, Java/.NET 업그레이드
- **Augment Code**: 전체 코드베이스 이해, Context Engine
- **Google Antigravity**: Windsurf 인수 기반, 자율 에이전트 (초기)

### 9.2 특수 목적
- **[Trail of Bits Security Skills](https://github.com/trailofbits)**: CodeQL/Semgrep 기반 보안 분석
- **[SkillsMP](https://skillsmp.com)**: 71K+ 에이전트 스킬 마켓플레이스

---

## 문서 작성 우선순위

1. **PLAN.md** ✅ (현재 문서)
2. **01-overview.md** - 전체 그림 제공
3. **02-tools.md** - 도구 선택 도움
4. **03-setup.md** - 실질적 설정 가이드
5. **04-methodology.md** - 방법론 이해
6. **05-parallel.md** - 고급 사용법
7. **06-spec-driven.md** - SDD 심화
8. **07-tools-catalog.md** - 도구 종합 카탈로그 (신규)

---

## 참고 자료

### 공식 문서
- [Claude Code Best Practices](https://www.anthropic.com/engineering/claude-code-best-practices)
- [Claude Code Docs - Memory](https://code.claude.com/docs/en/memory)
- [Claude Code Docs - Hooks](https://code.claude.com/docs/en/hooks-guide)
- [Claude Code Docs - MCP](https://code.claude.com/docs/en/mcp)
- [OpenCode](https://opencode.ai/)
- [GitHub Copilot Agent Mode](https://github.blog/ai-and-ml/github-copilot/agent-mode-101-all-about-github-copilots-powerful-mode/)
- [moai-adk](https://github.com/modu-ai/moai-adk)

### 커뮤니티 프로젝트
- [SuperClaude Framework](https://github.com/SuperClaude-Org/SuperClaude_Framework) (19K+ stars)
- [oh-my-claude-sisyphus](https://github.com/Yeachan-Heo/oh-my-claude-sisyphus)
- [oh-my-opencode](https://github.com/code-yeongyu/oh-my-opencode)
- [claude-mem](https://github.com/thedotmack/claude-mem)
- [cc-sdd](https://github.com/gotalab/cc-sdd)
- [vibe-tools](https://github.com/eastlondoner/vibe-tools)

### 오픈소스 에이전트
- [SWE-agent](https://github.com/SWE-agent/SWE-agent)
- [Devika](https://github.com/stitionai/devika)
- [GPT Engineer](https://github.com/gpt-engineer-org/gpt-engineer)
- [MetaGPT](https://github.com/geekan/MetaGPT)
- [Aider](https://github.com/paul-gauthier/aider)
- [Cline](https://github.com/cline/cline)
- [Plandex](https://github.com/plandex-ai/plandex)

### 큐레이션 목록
- [awesome-claude-code (hesreallyhim)](https://github.com/hesreallyhim/awesome-claude-code) (21K+ stars)
- [awesome-claude-code (jqueryscript)](https://github.com/jqueryscript/awesome-claude-code)
- [awesome-claude-plugins](https://github.com/quemsah/awesome-claude-plugins)
- [awesome-ai-agents](https://github.com/e2b-dev/awesome-ai-agents)
- [awesome-ai-devtools](https://github.com/jamesmurdza/awesome-ai-devtools)

### 분석/가이드
- [Writing a good CLAUDE.md](https://www.humanlayer.dev/blog/writing-a-good-claude-md)
- [Complete Guide to CLAUDE.md](https://www.builder.io/blog/claude-md-guide)
- [Claude Code vs Cursor](https://northflank.com/blog/claude-code-vs-cursor-comparison)
- [Roo Code vs Cline](https://www.qodo.ai/blog/roo-code-vs-cline/)
- [Best AI Coding Agents 2026](https://www.faros.ai/blog/best-ai-coding-agents-2026)
- [SDD Tools Analysis](https://martinfowler.com/articles/exploring-gen-ai/sdd-3-tools.html)

### 설문/통계
- [Stack Overflow 2025 Developer Survey - AI](https://survey.stackoverflow.co/2025/ai)
- [Sonar 2026 State of Code Report](https://www.sonarsource.com/state-of-code-developer-survey-report.pdf)
- [JetBrains State of Developer Ecosystem 2025](https://blog.jetbrains.com/research/2025/10/state-of-developer-ecosystem-2025/)

---

## 변경 이력
- 2026-01-20: 초안 작성
- 2026-01-20: 도구 종합 목록 추가 (섹션 8-9)
- 2026-01-21: Amp Code 추가, Claude Code 상세 가이드 분리 (docs/claude-code.md)
