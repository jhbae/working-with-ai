# AI 코딩 도구 현황 및 분류

---

## 1. 채택 현황 (2025-2026)

### 전체 사용률
| 지표 | 수치 | 출처 |
|------|------|------|
| AI 도구 사용 개발자 | 85% | Stack Overflow 2025 |
| 에이전트 미사용/간단 도구만 | 52% | - |
| Agentic AI 정기 사용자 | 25% | - |
| "Vibe coding" 전문 업무 미사용 | 72% | - |

### 도구별 채택률
| 도구 | 채택률 | 비고 |
|------|--------|------|
| ChatGPT | 82% | 범용, 코드 외 작업 포함 |
| GitHub Copilot | 68% | 자동완성 중심 |
| Claude Code | 45% | 전문 개발자 기준 |
| Cursor | 19.3% | Agentic 사용자 기준 |

### 신뢰와 우려
- **87%** 정확도 우려
- **81%** 보안/프라이버시 우려
- 도구를 "가속기"로 사용 vs "위임자"로 사용하는 차이

---

## 2. 도구 분류 체계

### 자율성 스펙트럼

**낮음 ←――――――――――――――――――――――→ 높음**

| 자동완성 | IDE 통합 | CLI 에이전트 | 오케스트레이터 |
|----------|----------|--------------|----------------|
| Copilot | Cursor | Claude Code | SuperClaude |
| Tabnine | Windsurf | Gemini CLI | oh-my-* |
| | Cline | OpenCode | moai-adk |
| | | Aider, Codex | Kiro |

### 분류 설명

#### 자동완성
- 코드 입력 중 다음 줄/블록 제안
- 가장 낮은 학습 곡선
- 예: GitHub Copilot, Tabnine

#### IDE 통합
- 에디터 내에서 채팅/편집 지원
- 파일 컨텍스트 인식
- 예: Cursor, Windsurf, Cline

#### CLI 에이전트
- 터미널에서 동작
- 멀티파일 작업, 대규모 컨텍스트
- 예: Claude Code, Gemini CLI, Aider

#### 오케스트레이터
- 여러 에이전트/모델 조율
- 복잡한 워크플로우 자동화
- 예: SuperClaude, oh-my-claude-sisyphus, moai-adk

---

## 3. 플랫폼별 분류

### 공식/상용 (Big Tech)
| 제공자 | 도구 | 특징 |
|--------|------|------|
| Anthropic | Claude Code | 200K 컨텍스트, 멀티파일 |
| Google | Gemini CLI | Conductor, 무료 티어 |
| OpenAI | Codex CLI | GPT-4.1, 샌드박스 |
| GitHub | Copilot | 68% 채택, Agent Mode |
| Microsoft | Cursor | 벡터 기반, 체크포인트 |

### 오픈소스
| 도구 | Stars | 특징 |
|------|-------|------|
| Aider | 25K+ | Git 통합, diff 기반 |
| Cline | 40K+ | VS Code, Plan/Act |
| Continue | 25K+ | 엔터프라이즈, 셀프호스트 |
| Tabby | 25K+ | 셀프호스트 Copilot 대안 |

### 앱 빌더 (Vibe Coding)
| 도구 | 특징 | 적합 |
|------|------|------|
| Lovable | $100M ARR, Supabase 통합 | MVP |
| v0 (Vercel) | React/Next.js 특화 | 프론트엔드 |
| Bolt.new | 브라우저, WebContainer | 프로토타입 |
| Replit Agent | 다국어, 풀스택 | 학습 |

---

## 4. 선택 가이드

### 상황별 추천

| 상황 | 추천 도구 | 이유 |
|------|-----------|------|
| 입문자 | Copilot, Cursor | 낮은 학습 곡선 |
| 대규모 리팩토링 | Claude Code | 200K 컨텍스트 |
| 비용 민감 | OpenCode, Aider | 오픈소스, BYOK |
| 프라이버시 중시 | Tabby, Continue | 셀프호스트 |
| 빠른 프로토타입 | Lovable, Bolt.new | Vibe coding |
| 팀 협업 | Amp Code | 스레드 공유 |

### 비용 비교

| 도구 | 가격 | 포함 내용 |
|------|------|-----------|
| GitHub Copilot | $10-19/월 | 자동완성, Agent Mode |
| Cursor Pro | $20/월 | IDE 에이전트 |
| Claude Pro | $20/월 | API 크레딧 |
| Claude Max | $100-200/월 | 대용량 사용 |
| Windsurf | $15/월 | IDE 에이전트 |
| OpenCode | 무료 | BYOK (API 비용 별도) |

---

## 다음 문서

- [02-tools.md](02-tools.md) - 도구 상세 비교
- [03-methodology.md](03-methodology.md) - 개발 방법론
