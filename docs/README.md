# AI 기반 개발 도구 & 방법론 종합 가이드

AI 코딩 도구들의 현황, 설정 방법, 개발 방법론을 **실용적 관점**에서 정리한 가이드.

---

## 문서 구조

| 문서 | 내용 |
|------|------|
| [01-overview.md](01-overview.md) | AI 코딩 도구 현황, 채택률, 분류 체계 |
| [02-tools.md](02-tools.md) | 도구 종합 비교 (CLI, IDE, 오픈소스) |
| [03-methodology.md](03-methodology.md) | 개발 방법론 (SDD, Agentic Patterns) |
| [04-claude-code.md](04-claude-code.md) | Claude Code 완전 가이드 |
| [05-parallel.md](05-parallel.md) | 병렬 작업 & 오케스트레이션 |
| [references.md](references.md) | 참고 자료 모음 |

---

## 빠른 시작

### 1. 입문자
```
GitHub Copilot ($10-19/월) 또는 Cursor ($20/월)
→ IDE 통합, 자동완성 중심, 학습 곡선 낮음
```

### 2. 중급자
```
Claude Code 추가 ($20-200/월)
→ 대규모 리팩토링, 복잡한 버그, 멀티파일 작업
→ 터미널 기반, 200K 컨텍스트
```

### 3. 고급 사용자
```
필요 시 검토:
- SuperClaude: 16개 페르소나, 30개 명령어
- moai-adk: SPEC-First DDD, 한국어 지원
- oh-my-claude-sisyphus: 멀티 에이전트 오케스트레이션
```

---

## 실용적 권장사항

### 현실적 기대치
- AI 도구는 **가속기**지 **대체재**가 아님
- 코드 리뷰는 여전히 필수
- 87%가 정확도 우려 → 검증 필수

### 비용 대비 효과 (가성비 순)
| 순위 | 도구 | 가격 | 용도 |
|------|------|------|------|
| 1 | GitHub Copilot | $10-19/월 | 일상 자동완성 |
| 2 | Cursor Pro | $20/월 | IDE 통합 에이전트 |
| 3 | Claude Pro | $20/월 | 복잡한 작업 escalation |
| 4 | Claude Max | $100-200/월 | 대규모 프로젝트 |

### 작업 복잡도별 접근법

| 간단한 수정 | 중간 기능 | 복잡한 시스템 |
|-------------|-----------|---------------|
| 직접 코딩 | Plan Mode | Spec-Driven |
| 자동완성 | 구조화된 프롬프트 | 문서 선행 |
| Copilot, Cursor | Claude Code, /plan | Kiro, moai-adk |

---

## 핵심 인사이트

### Boris Cherny (Claude Code 창시자)
> "Claude에게 작업 검증 방법을 제공하면 최종 결과 품질이 2-3배 향상"

### Addy Osmani
> "더 많은 컨텍스트 = 더 나은 결과"가 **아님**. 필요한 정보만 선택적 제공.

### Agentic Patterns
> Rich Feedback > Perfect Prompts. 완벽한 프롬프트보다 풍부한 피드백이 효과적.

---

## 통계 (2025-2026)

- 85% 개발자가 AI 도구 사용 (Stack Overflow 2025)
- 52%는 에이전트 미사용 또는 간단한 도구만
- Agentic AI 정기 사용자는 25%
- "Vibe coding"은 72%가 전문 업무에 미사용

---

*마지막 업데이트: 2026-01-22*
