# AI 코딩 도구 종합 비교

> GitHub Stars 1K+ 기준, 2026년 1월 기준

---

## 1. 공식/주류 도구

### CLI 에이전트 (Big Tech)

| 도구 | Stars | 제공자 | 특징 |
|------|-------|--------|------|
| **[Claude Code](https://claude.ai/code)** | 55K+ | Anthropic | Opus 4.6 Agent Teams, 200K 컨텍스트 (1M 베타), MCP 지원 → [상세 가이드](04-claude-code.md) |
| **[Gemini CLI](https://github.com/google-gemini/gemini-cli)** | 90K+ | Google | 1M 컨텍스트, 무료 티어 (60req/min) |
| **[Codex CLI](https://github.com/openai/codex)** | 15K+ | OpenAI | GPT-4.1, 샌드박스 실행 |

#### Claude Code
- **강점**: 멀티파일 작업, 실제 200K 컨텍스트, 30% 적은 재작업
- **약점**: 비용 ($100-200 Max), 터미널 기반
- **적합**: 대규모 리팩토링, 복잡한 버그, 설계 수준 변경

#### Gemini CLI
- **강점**: Google 생태계 통합, 1M 토큰 컨텍스트, 무료 티어
- **약점**: Claude/GPT 대비 코딩 성능 논쟁
- **적합**: Google Cloud 사용자, 비용 민감 개발자

#### Codex CLI
- **강점**: OpenAI 생태계, GPT-4.1 최신 모델
- **약점**: 상대적으로 신생
- **적합**: OpenAI API 사용자

### IDE 통합

| 도구 | 특징 | 가격 |
|------|------|------|
| **GitHub Copilot** | 68% 채택률, Agent Mode, MCP 지원 | $10-19/월 |
| **Cursor** | Copilot fork, 벡터 기반 이해, 체크포인트 | $20/월 |
| **Windsurf** | Wave 13: 병렬 멀티에이전트, Agent Skills, SWE-1.5 무료 | $15/월 |
| **[Warp](https://warp.dev)** | AI 터미널, 멀티 에이전트, Full Terminal Use | 무료 + Pro |
| **[Amp Code](https://ampcode.com)** | Sourcegraph, 멀티모델 자동 라우팅, 서브에이전트 병렬, Skills 호환 | $10/일 무료 + 종량제 |
| **[Copilot Workspace](https://githubnext.com/projects/copilot-workspace)** | Agents HQ: Claude+Codex 병렬 실행, 이슈→PR 자동화 | Pro+/Enterprise |

#### Warp
- **강점**: 터미널 자체가 AI 에이전트, Full Terminal Use (REPL/디버거 조작), MCP 통합
- **약점**: macOS/Linux만 (Windows 제한적)
- **적합**: 터미널 헤비유저, CLI 작업 중심 개발자

#### Amp Code vs Claude Code
| 측면 | Claude Code | Amp Code |
|------|-------------|----------|
| 모델 | Claude만 | Claude + GPT + Gemini (자동 선택) |
| 자동 작업 분배 | Agent Teams (Opus 4.6) | 서브에이전트 병렬 실행 |
| 비용 | $200/월 고정 (Max) | 종량제, 무료 $10/일 (~$300/월) |
| 팀 협업 | 기본 | 스레드 공유, 리더보드 |
| Skills | 원본 | Claude Skills 호환 |
| 레이트 리밋 | 있음 (5시간 윈도우) | 없음 |

> "Claude Code를 이긴 첫 번째 도구" - Glen Maddern, Cloudflare

---

## 2. CLI 에이전트 (오픈소스)

| 도구 | Stars | 특징 |
|------|-------|------|
| **[OpenClaw](https://openclaw.ai)** | 100K+ | 모델 불가지론(Claude/OpenAI/KIMI), 로컬 실행, 메시징 통합 |
| **[Aider](https://github.com/Aider-AI/aider)** | 40K+ | Git 통합, diff 기반 편집, 멀티 LLM |
| **[OpenCode](https://github.com/anomalyco/opencode)** | 70K+ | BYOK, 병렬 세션, LSP 통합, GitHub Copilot 공식 지원 |
| **[Plandex](https://github.com/plandex-ai/plandex)** | 활발 | 2M 토큰 컨텍스트, 30+ 언어 |
| **[Qodo Command](https://www.qodo.ai)** | 활발 | PR 리뷰, 테스트 감사, CI/CD 통합, 멀티 LLM |
| **[OpenHands](https://openhands.dev)** | 60K+ | $18.8M 시리즈A, 87% 버그 당일 해결, AMD/Apple/Google 사용 |

#### OpenClaw (구 Clawdbot → Moltbot)
- **강점**: 모델 불가지론(Claude, OpenAI, KIMI, MiMo 등), 로컬 실행, 메시징 플랫폼 통합
- **약점**: 보안 우려 (샌드박스 권장), 이름 변경 혼란
- **적합**: 다양한 모델 사용자, 프라이버시 중시
- **참고**: Anthropic 상표권 요청으로 Clawdbot → Moltbot 변경 후, 크립토 스캐머 사건으로 OpenClaw로 최종 변경

#### Aider
- **강점**: Git 통합, 투명한 diff 기반 편집
- **약점**: 설정 복잡도
- **적합**: Git 워크플로우 중시, 명시적 제어 선호

#### Aider 실사용 주의점
- **수동 파일 선택 필수**: Claude Code/OpenCode와 달리 파일을 직접 추가해야 함
- **대형 코드베이스 한계**: 컨텍스트 윈도우 초과 시 성능 급락
- **원치 않는 편집**: 질문만 하려는데 코드를 수정하고 커밋하는 사례 빈번

#### OpenCode
- **강점**: 오픈소스, BYOK(모델 자유 선택), 병렬 세션, LSP 통합
- **약점**: 상대적으로 신생
- **적합**: 비용 민감, 프라이버시 중시, 온프레미스 필요

#### OpenCode 실사용 주의점
- **보안**: CVE-2026-22812 (CVSS 8.8) RCE 취약점 발견, v1.0.216+에서 패치
- **안정성**: 허락 없는 코드 리포맷, 테스트 삭제 사례 보고
- **Git 부하**: 세션 스냅샷으로 대형 코드베이스에서 CPU 과부하 발생

---

## 3. IDE/에디터 통합 (오픈소스)

| 도구 | Stars | 특징 |
|------|-------|------|
| **[Cline](https://github.com/cline/cline)** | 52K+ | VS Code, Plan/Act 모드, 자율 에이전트 |
| **[Roo Code](https://github.com/RooVetGit/Roo-Cline)** | 10K+ | VS Code, 멀티 에이전트, 역할 기반 |
| **[Continue](https://github.com/continuedev/continue)** | 25K+ | VS Code/JetBrains, 엔터프라이즈, 셀프호스트 |
| **[Tabby](https://github.com/TabbyML/tabby)** | 25K+ | 셀프호스트 Copilot 대안 |
| **[Kilo Code](https://kilocode.ai)** | 활발 | 구조화된 모드, 컨텍스트 관리 강화, 환각 감소 |
| **[Cody](https://sourcegraph.com/cody)** | - | Sourcegraph, 대형 모노레포 특화 |
| **[Zed AI](https://zed.dev/ai)** | 55K+ | 고성능 에디터, ACP 프로토콜 |

---

## 4. 오픈소스 AI 에이전트

| 도구 | Stars | 특징 |
|------|-------|------|
| **[GPT Engineer](https://github.com/gpt-engineer-org/gpt-engineer)** | 55K+ | 전체 코드베이스 생성 |
| **[MetaGPT](https://github.com/geekan/MetaGPT)** | 50K+ | 멀티 에이전트, PRD→코드 |
| **[Goose](https://github.com/block/goose)** | 활발 | Block(Square) 제작, MCP 지원, Rust 기반, 데스크톱+CLI |
| **[Devika](https://github.com/stitionai/devika)** | 20K+ | Devin 대안, 웹 리서치 + 코딩 |
| **[SWE-agent](https://github.com/SWE-agent/SWE-agent)** | 15K+ | SWE-bench SOTA, NeurIPS 2024 |
| **[Smol Developer](https://github.com/smol-ai/developer)** | 12K+ | 경량 프로토타입 에이전트 |
| **[Grok Build](https://build.x.ai)** | 활발 | xAI, 자연어→소프트웨어, Grok Code Fast-1 모델 |

---

## 5. 앱 빌더 (Vibe Coding)

| 도구 | 특징 | 적합 |
|------|------|------|
| **[Lovable](https://lovable.dev)** | $100M ARR, Supabase 통합 | MVP, 사이드 프로젝트 |
| **[v0](https://v0.app)** (Vercel) | React/Next.js 특화, 에이전트 모드 | 프론트엔드 |
| **[Bolt.new](https://bolt.new)** | 브라우저 기반, WebContainer, 오픈소스 | 빠른 프로토타입 |
| **[Replit Agent v3](https://replit.com)** | 200분 자율 실행, 자가치유, 2-3x 속도 향상 | 학습, 실험, $100/월 Pro |

---

## 6. Spec-Driven 도구

| 도구 | 플랫폼 | 특징 | 성숙도 |
|------|--------|------|--------|
| **Kiro** (AWS) | IDE | spec/vibe 모드 전환, DevOps 자동화 | 초기 |
| **[GitHub Spec-Kit](https://github.com/github/spec-kit)** | CLI | 다양한 코딩 도구 지원 | 초기 |
| **[cc-sdd](https://github.com/gotalab/cc-sdd)** | 설정 | Claude Code/Cursor/Gemini 등 | 커뮤니티 |
| **[OpenSpec](https://github.com/Fission-AI/OpenSpec)** | 프레임워크 | 범용 SDD | 커뮤니티 |
| **[Tessl](https://tessl.io)** | 프레임워크 | 스펙 레지스트리 10K+, 57%→93% 성공률 개선 | 성장중 |

---

## 7. AI 코딩 모델 (로컬/오픈소스)

| 모델 | 제공자 | 특징 |
|------|--------|------|
| **Qwen3-Coder** | Alibaba | 오픈소스 SOTA, 256K 컨텍스트, SWE-bench 69.6% |
| **DeepSeek R1** | DeepSeek | 오픈소스 추론 모델, "DeepSeek moment" 촉발 |
| **DeepSeek Coder V2** | DeepSeek | 128K 컨텍스트, 338 언어, MoE 236B |
| **Devstral 2** | Mistral | 에이전틱 특화, SWE-bench 53.8%, IDE 통합 |
| **Codestral** | Mistral | 80+ 언어, HumanEval 86.6% |
| **StarCoder2** | BigCode | 오픈소스, 다양한 크기 |
| **GPT-5.3-Codex** | OpenAI | 최신 코딩 에이전트 모델, Terminal-Bench 77.3% |
| **Gemini 2.5 Flash/Pro** | Google | 고급 추론/코딩, 에이전틱 특화, 2.0 Flash 3/31 종료 |
| **DeepSeek-V3.1** | DeepSeek | 671B (37B 활성), 하이브리드 추론+코딩, 128K 컨텍스트 |
| **Grok Code Fast-1** | xAI | 경량 코딩 추론, Copilot/Cursor/Windsurf 등 무료 파트너십 |

---

## 8. 기업/상용 (기타)

| 도구 | 특징 |
|------|------|
| **Amazon Q Developer** | AWS 통합, 코드 리뷰, Java/.NET 업그레이드 |
| **Augment Code** | 전체 코드베이스 이해, Context Engine |
| **Google Antigravity** | Windsurf 인수 기반, 무료 AI IDE 프리뷰 (2026-01-08), 병렬 에이전트 오케스트레이션 |
| **[Devin](https://cognition.ai)** | Cognition AI, 자율 코딩 에이전트, Devin Review(PR 리뷰), Infosys 파트너십 |
| **[Factory.ai](https://factory.ai)** | "Droid" 에이전트, SWE-Bench 84.8%, Terminal-Bench SOTA, 이슈→PR 자동화 |

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

*마지막 업데이트: 2026-02-10*
