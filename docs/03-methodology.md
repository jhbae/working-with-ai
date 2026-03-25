# AI 개발 방법론

---

## 1. 방법론 개요

### 주요 방법론 비교

| 방법론 | 특징 | 적합 |
|--------|------|------|
| **Vibe Coding** | 프롬프트로 전체 앱 생성 | 프로토타입, 해커톤 |
| **Spec-Driven (SDD)** | 문서 선행, 명시적 요구사항 | 프로덕션, 팀 |
| **Context-Driven** | 컨텍스트 기반 점진적 개발 | 중간 규모 |
| **Plan-Then-Execute** | 계획 후 실행 | 복잡한 작업 |
| **Compound Engineering** | 계획 80%, 실행 20%, 복리형 자산화 | 팀, AI 네이티브 |
| **Context Engineering** | 최적 컨텍스트 큐레이션, 프롬프트 → 컨텍스트 전환 | AI 네이티브 팀 |

### 복잡도별 선택

| 간단한 수정 | 중간 기능 | 복잡한 시스템 |
|-------------|-----------|---------------|
| 직접 코딩 | Plan Mode | Spec-Driven / Compound |
| 자동완성 | 구조화된 프롬프트 | 문서 선행, 리뷰 에이전트 |
| Copilot, Cursor | Claude Code, /plan | Kiro, moai-adk, Tessl |

---

## 2. Vibe Coding

### 개요
- 프롬프트로 전체 앱 생성
- "감"으로 코딩, 빠른 반복

### 현실
- **72%가 전문 업무에 미사용**
- 재작업 많음, 품질 가변적
- Collins 올해의 단어 2025 선정
- 시장 규모: $81.4B → $127B (2032 전망)
- 코드의 **51%**가 GitHub 커밋에서 AI 생성/지원 (2026 초 기준)
- Y Combinator 2025 겨울 코호트의 25%가 95% AI 생성 코드베이스로 출시 → 기술 부채/보안 이슈 발생
- Lovable $100M ARR 달성, 비개발자 앱 제작 현실화

### 적합한 상황
- 프로토타입
- 학습, 실험
- 해커톤
- 개인 프로젝트

### 도구
- Lovable, v0, Bolt.new, Replit Agent

---

## 3. Spec-Driven Development (SDD)

### 개요
```
requirements.md → design.md → tasks.md → 구현
```
- 에이전트 행동의 "진실의 원천"
- 문서가 코드보다 선행

### 장점
- 명시적 요구사항
- 재작업 감소
- 팀 협업 용이
- 생산성 향상: 적절 구현 시 30-35% (출처: McKinsey 20-45% 범위)

### 현실
- 67% 팀이 학습 기간 추가 디버깅 경험, ROI 실현까지 3-6개월
- 15+ SDD 플랫폼 출시 (2024-2025), GitHub Spec Kit 72K+ stars

### 도구
- Kiro (AWS)
- GitHub Spec-Kit
- cc-sdd
- moai-adk
- Tessl Framework (스펙 레지스트리 10K+, 57%→93% 성공률 개선)

### SDD vs Vibe Coding

| 측면 | Spec-Driven | Vibe Coding |
|------|-------------|-------------|
| 속도 | 느림 (문서화 선행) | 빠름 |
| 품질 | 높음 (명시적 요구사항) | 가변적 |
| 재작업 | 적음 | 많음 |
| 적합 | 프로덕션, 팀 | 프로토타입, 개인 |

### moai-adk 워크플로우 예시
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

---

## 4. AGENTS.md 표준

### 개요
에이전트가 프로젝트를 이해하도록 돕는 마크다운 기반 표준.

```
repository-root/
└── AGENTS.md    # 프로젝트별 에이전트 지침
```

### 현황
- **60K+** 오픈소스 프로젝트 채택 (2026-02 기준)
- OpenAI Codex, GitHub Copilot, Cursor, Gemini CLI 지원
- Linux Foundation의 **Agentic AI Foundation**에서 거버넌스

### CLAUDE.md vs AGENTS.md

| 측면 | CLAUDE.md | AGENTS.md |
|------|-----------|-----------|
| 대상 | Claude Code 전용 | 모든 에이전트 범용 |
| 거버넌스 | Anthropic | Linux Foundation (AAIF) |
| 호환성 | Claude Code | Codex, Copilot, Cursor 등 |

> 참고: [agents.md](https://agents.md/)

---

## 5. Compound Engineering (복리형 개발)

> 출처: [Compound Engineering](https://every.to/guides/compound-engineering)

### 개요
모든 엔지니어링 작업이 다음 작업을 더 쉽게 만드는 방법론. "계획이 새로운 코드다."

```
Plan (40%) → Work (10%) → Review (40%) → Compound (10%)
```

### 기존 방법론과의 차이

| 측면 | Vibe Coding | SDD | Compound |
|------|-------------|-----|----------|
| 계획 비중 | 낮음 | 높음 (문서 선행) | **80%** |
| AI 역할 | 전체 생성 | 스펙 기반 구현 | 구현 + 리뷰 |
| 품질 보장 | 수동 | 테스트 | **14+ 리뷰 에이전트** |
| 학습 축적 | 없음 | 스펙 재사용 | **패턴 자산화** |
| 적합 | 프로토타입 | 프로덕션 | **팀, 장기 프로젝트** |

### 핵심 원칙
- 엔지니어 시간의 80%를 계획과 리뷰에 투자
- 14+ 전문 리뷰 에이전트가 보안/성능/아키텍처 병렬 검토
- 해결한 패턴을 시스템에 인코딩하여 미래 작업 자동화
- "산출물은 키 입력이 아닌 해결한 문제로 측정"

### 2026년 "품질의 해" 전환

> 출처: [CodeRabbit](https://www.coderabbit.ai/blog/2025-was-the-year-of-ai-speed-2026-will-be-the-year-of-ai-quality)

2025년이 속도의 해였다면, 2026년은 품질의 해.

| 지표 | 기존 | 2026 전환 |
|------|------|-----------|
| 품질 측정 | 코드 커버리지 | 변이 테스트 (Mutation Testing) |
| 리뷰 방식 | 인간 리뷰 | AI-on-AI 리뷰 + 인간 감독 |
| 결함 추적 | 버그 수 | 결함 밀도, 인시던트 심각도 |
| 보안 | 수동 감사 | 자동화된 OWASP 스캔 + AI 리뷰 |

---

## 6. Context Engineering (컨텍스트 엔지니어링)

> 출처: [Effective Context Engineering for AI Agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)

### 개요
프롬프트 엔지니어링에서 진화한 개념. LLM 추론 시 최적의 토큰 세트를 큐레이션하고 유지하는 방법론.

AI를 상태 없는 계산기에서 상태 인식, 메모리 기반 파트너로 전환.

### 핵심 전략

| 전략 | 설명 |
|------|------|
| **선택(Selection)** | 관련 정보만 포함, 불필요한 컨텍스트 제거 |
| **압축(Compression)** | 토큰 예산 내에서 정보 밀도 최대화 |
| **순서(Ordering)** | 중요 정보의 위치 최적화 (시작/끝 우선) |
| **격리(Isolation)** | 작업별 컨텍스트 분리로 오염 방지 |
| **형식(Format)** | 구조화된 형식 (마크다운, JSON)으로 파싱 최적화 |

### 프롬프트 엔지니어링과의 차이

| 측면 | Prompt Engineering | Context Engineering |
|------|-------------------|---------------------|
| 초점 | 질문 방법 | 무엇을 제공하는가 |
| 범위 | 단일 프롬프트 | 전체 토큰 윈도우 관리 |
| 상태 | 상태 없음 | 상태 인식, 메모리 기반 |
| 성공 요인 | 영리한 프롬프트 | 우수한 컨텍스트 큐레이션 |

### 실전 적용
- CLAUDE.md / AGENTS.md가 대표적 컨텍스트 엔지니어링 도구
- MCP 서버를 통한 외부 컨텍스트 자동 주입
- /compact로 컨텍스트 압축, 세션 간 메모리 유지
- 팀에서 공유 컨텍스트 파일 운영 (git 커밋)

> "신뢰할 수 있는 AI 코드를 배포하는 팀의 차이점은 영리한 프롬프트가 아니라 우수한 컨텍스트 엔지니어링에 있다."

---

## 다음 문서

- [04-claude-code.md](04-claude-code.md) - Claude Code 상세
- [05-parallel.md](05-parallel.md) - 병렬 작업 & 오케스트레이션
- [06-practices.md](06-practices.md) - 실전 기법 & 패턴

---

*마지막 업데이트: 2026-03-25*
