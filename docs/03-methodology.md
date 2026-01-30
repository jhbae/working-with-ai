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

### 복잡도별 선택

| 간단한 수정 | 중간 기능 | 복잡한 시스템 |
|-------------|-----------|---------------|
| 직접 코딩 | Plan Mode | Spec-Driven |
| 자동완성 | 구조화된 프롬프트 | 문서 선행 |
| Copilot, Cursor | Claude Code, /plan | Kiro, moai-adk |

---

## 2. Vibe Coding

### 개요
- 프롬프트로 전체 앱 생성
- "감"으로 코딩, 빠른 반복

### 현실
- **72%가 전문 업무에 미사용**
- 재작업 많음, 품질 가변적

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

## 다음 문서

- [04-claude-code.md](04-claude-code.md) - Claude Code 상세
- [05-parallel.md](05-parallel.md) - 병렬 작업 & 오케스트레이션
- [06-practices.md](06-practices.md) - 실전 기법 & 패턴

---

*마지막 업데이트: 2026-01-30*
