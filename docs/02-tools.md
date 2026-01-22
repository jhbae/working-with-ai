# AI 코딩 도구 종합 비교

> GitHub Stars 1K+ 기준, 2026년 1월 기준

---

## 1. 공식/주류 도구

### CLI 에이전트 (Big Tech)

| 도구 | Stars | 제공자 | 특징 |
|------|-------|--------|------|
| **[Claude Code](https://claude.ai/code)** | 55K+ | Anthropic | 200K 컨텍스트, 멀티파일, MCP 지원 → [상세 가이드](04-claude-code.md) |
| **[Gemini CLI](https://github.com/google-gemini/gemini-cli)** | 50K+ | Google | Conductor, 컨텍스트 기반, 무료 티어 |
| **[Codex CLI](https://github.com/openai/codex)** | 15K+ | OpenAI | GPT-4.1, 샌드박스 실행 |

#### Claude Code
- **강점**: 멀티파일 작업, 실제 200K 컨텍스트, 30% 적은 재작업
- **약점**: 비용 ($100-200 Max), 터미널 기반
- **적합**: 대규모 리팩토링, 복잡한 버그, 설계 수준 변경

#### Gemini CLI
- **강점**: Google 생태계 통합, Conductor로 컨텍스트 기반 개발
- **약점**: Claude/GPT 대비 코딩 성능 논쟁
- **적합**: Google Cloud 사용자, 실험적 사용

#### Codex CLI
- **강점**: OpenAI 생태계, GPT-4.1 최신 모델
- **약점**: 상대적으로 신생
- **적합**: OpenAI API 사용자

### IDE 통합

| 도구 | 특징 | 가격 |
|------|------|------|
| **GitHub Copilot** | 68% 채택률, Agent Mode, MCP 지원 | $10-19/월 |
| **Cursor** | Copilot fork, 벡터 기반 이해, 체크포인트 | $20/월 |
| **Windsurf** | Codeium, Riptide 검색 | $15/월 |
| **[Amp Code](https://ampcode.com)** | Sourcegraph, 멀티모델, Claude Skills 호환 | $10/일 무료 + 유료 |

#### Amp Code vs Claude Code
| 측면 | Claude Code | Amp Code |
|------|-------------|----------|
| 모델 | Anthropic 위주 | 멀티모델 (유연함) |
| 팀 협업 | 기본 | 스레드/컨텍스트 공유 기본 |
| Skills | 원본 | Claude Skills 호환 |
| 모드 | 단일 | Smart/Rush (비용 최적화) |

> "Claude Code를 이긴 첫 번째 도구" - Glen Maddern, Cloudflare

---

## 2. CLI 에이전트 (오픈소스)

| 도구 | Stars | 특징 |
|------|-------|------|
| **[Aider](https://github.com/paul-gauthier/aider)** | 25K+ | Git 통합, diff 기반 편집, 멀티 LLM |
| **[OpenCode](https://github.com/opencode-ai/opencode)** | 활발 | BYOK, 병렬 세션, LSP 통합, 온프레미스 |
| **[Plandex](https://github.com/plandex-ai/plandex)** | 활발 | 2M 토큰 컨텍스트, 30+ 언어 |

#### Aider
- **강점**: Git 통합, 투명한 diff 기반 편집
- **약점**: 설정 복잡도
- **적합**: Git 워크플로우 중시, 명시적 제어 선호

#### OpenCode
- **강점**: 오픈소스, BYOK(모델 자유 선택), 병렬 세션, LSP 통합
- **약점**: 상대적으로 신생
- **적합**: 비용 민감, 프라이버시 중시, 온프레미스 필요

---

## 3. IDE/에디터 통합 (오픈소스)

| 도구 | Stars | 특징 |
|------|-------|------|
| **[Cline](https://github.com/cline/cline)** | 40K+ | VS Code, Plan/Act 모드, 자율 에이전트 |
| **[Roo Code](https://github.com/RooVetGit/Roo-Cline)** | 10K+ | VS Code, 멀티 에이전트, 역할 기반 |
| **[Continue](https://github.com/continuedev/continue)** | 25K+ | VS Code/JetBrains, 엔터프라이즈, 셀프호스트 |
| **[Tabby](https://github.com/TabbyML/tabby)** | 25K+ | 셀프호스트 Copilot 대안 |
| **[Cody](https://sourcegraph.com/cody)** | - | Sourcegraph, 대형 모노레포 특화 |
| **[Zed AI](https://zed.dev/ai)** | 55K+ | 고성능 에디터, ACP 프로토콜 |

---

## 4. 오픈소스 AI 에이전트

| 도구 | Stars | 특징 |
|------|-------|------|
| **[GPT Engineer](https://github.com/gpt-engineer-org/gpt-engineer)** | 55K+ | 전체 코드베이스 생성 |
| **[MetaGPT](https://github.com/geekan/MetaGPT)** | 50K+ | 멀티 에이전트, PRD→코드 |
| **[Devika](https://github.com/stitionai/devika)** | 20K+ | Devin 대안, 웹 리서치 + 코딩 |
| **[SWE-agent](https://github.com/SWE-agent/SWE-agent)** | 15K+ | SWE-bench SOTA, NeurIPS 2024 |
| **[Smol Developer](https://github.com/smol-ai/developer)** | 12K+ | 경량 프로토타입 에이전트 |

---

## 5. 앱 빌더 (Vibe Coding)

| 도구 | 특징 | 적합 |
|------|------|------|
| **[Lovable](https://lovable.dev)** | $100M ARR, Supabase 통합 | MVP, 사이드 프로젝트 |
| **[v0](https://v0.app)** (Vercel) | React/Next.js 특화, 에이전트 모드 | 프론트엔드 |
| **[Bolt.new](https://bolt.new)** | 브라우저 기반, WebContainer, 오픈소스 | 빠른 프로토타입 |
| **[Replit Agent](https://replit.com)** | 다국어, Plan/Build 모드, 풀스택 | 학습, 실험 |

---

## 6. Spec-Driven 도구

| 도구 | 플랫폼 | 특징 | 성숙도 |
|------|--------|------|--------|
| **Kiro** (AWS) | IDE | spec/vibe 모드 전환, DevOps 자동화 | 초기 |
| **[GitHub Spec-Kit](https://github.com/github/spec-kit)** | CLI | 다양한 코딩 도구 지원 | 초기 |
| **[cc-sdd](https://github.com/gotalab/cc-sdd)** | 설정 | Claude Code/Cursor/Gemini 등 | 커뮤니티 |
| **[OpenSpec](https://github.com/Fission-AI/OpenSpec)** | 프레임워크 | 범용 SDD | 커뮤니티 |

---

## 7. AI 코딩 모델 (로컬/오픈소스)

| 모델 | 제공자 | 특징 |
|------|--------|------|
| **Qwen3-Coder** | Alibaba | 오픈소스 SOTA, 256K 컨텍스트, SWE-bench 69.6% |
| **DeepSeek Coder V2** | DeepSeek | 128K 컨텍스트, 338 언어, MoE 236B |
| **Codestral** | Mistral | 80+ 언어, HumanEval 86.6% |
| **StarCoder2** | BigCode | 오픈소스, 다양한 크기 |

---

## 8. 기업/상용 (기타)

| 도구 | 특징 |
|------|------|
| **Amazon Q Developer** | AWS 통합, 코드 리뷰, Java/.NET 업그레이드 |
| **Augment Code** | 전체 코드베이스 이해, Context Engine |
| **Google Antigravity** | Windsurf 인수 기반, 자율 에이전트 (초기) |

---

## 9. 큐레이션 목록 (Awesome Lists)

| 목록 | Stars | 설명 |
|------|-------|------|
| **[awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code)** | 21K+ | 스킬, 훅, 명령어 종합 |
| **[awesome-claude-plugins](https://github.com/quemsah/awesome-claude-plugins)** | 활발 | 243개 플러그인 |
| **[awesome-ai-agents](https://github.com/e2b-dev/awesome-ai-agents)** | 15K+ | AI 에이전트 종합 |
| **[awesome-ai-devtools](https://github.com/jamesmurdza/awesome-ai-devtools)** | 활발 | AI 개발 도구 전반 |
| **[500-AI-Agents-Projects](https://github.com/ashishpatel26/500-AI-Agents-Projects)** | 활발 | 500+ 에이전트 프로젝트 |

---

## 다음 문서

- [03-methodology.md](03-methodology.md) - 개발 방법론
- [04-claude-code.md](04-claude-code.md) - Claude Code 상세
- [05-parallel.md](05-parallel.md) - 오케스트레이터 상세
