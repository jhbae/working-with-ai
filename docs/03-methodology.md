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
- 코드의 41%가 AI 생성 (2025 기준)

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

---

## 다음 문서

- [04-claude-code.md](04-claude-code.md) - Claude Code 상세
- [05-parallel.md](05-parallel.md) - 병렬 작업 & 오케스트레이션
- [06-practices.md](06-practices.md) - 실전 기법 & 패턴

---

*마지막 업데이트: 2026-02-10*
