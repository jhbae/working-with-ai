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

## 4. 좋은 스펙 작성법 (Addy Osmani)

> 출처: [Writing Good Specs for AI Agents](https://addyosmani.com/blog/good-spec/)

### 5가지 핵심 원칙

#### 1. 고수준 비전부터 시작
```markdown
# 잘못된 접근
"다음은 모든 요구사항이에요: 인증, 결제, 이메일..."

# 올바른 접근
"사용자가 작업을 추가/편집/완료할 수 있는 웹 앱 구축.
사용자 계정, 데이터베이스, 반응형 UI 필요"
→ AI가 상세 스펙 작성하도록 요청
```

#### 2. PRD 구조화 (6가지 필수 영역)
```markdown
# 프로젝트 스펙

## 기술 스택
React 18+, TypeScript, Vite, Tailwind CSS

## 명령어
- Build: `npm run build`
- Test: `npm test`
- Lint: `npm run lint --fix`

## 테스트
- Framework: Vitest
- 위치: `tests/`
- 커버리지: 80% 이상

## 프로젝트 구조
src/, tests/, docs/

## 코드 스타일
[설명보다 실제 코드 스니펫이 효과적]

## 경계 (중요!)
✅ 항상: 테스트 실행, 명명 규칙 준수
⚠️ 먼저 묻기: DB 스키마 변경, 의존성 추가
🚫 절대 금지: 시크릿 커밋, node_modules 수정
```

#### 3. 작은 단위 태스크로 분할

> "지시사항의 저주": 요구사항이 많을수록 각각의 준수율 급락

```
Step 1: DB 스키마 구현
Step 2: 인증 기능
Step 3: UI 컴포넌트
→ 순차적 포커스
```

| 방식 | 장점 | 적합 |
|------|------|------|
| 단일 에이전트 | 간단한 설정 | 중소 프로젝트 |
| 멀티 에이전트 | 높은 처리량 | 대규모 코드베이스 |

#### 4. 3단계 경계 시스템
```
✅ 항상 실행 (승인 불필요)
   "항상 커밋 전 테스트 실행"

⚠️ 먼저 문의 (검토 필요)
   "DB 스키마 변경 전 확인"

🚫 절대 금지 (하드스탑)
   "절대 시크릿이나 API 키 커밋 금지"
```

#### 5. 테스트, 반복, 진화
- 각 마일스톤 후 테스트 실행
- 실패 시 스펙 업데이트
- Git으로 스펙도 버전 관리
- "LLM as Judge": 두 번째 AI가 스펙 준수 여부 평가

### 피해야 할 함정

| 함정 | 문제 | 해결 |
|------|------|------|
| 모호한 스펙 | "좋게 만들어" | "추가/편집/삭제 가능, 반응형" |
| 과도한 컨텍스트 | 50페이지 문서 | 계층적 요약, 토큰 예산 관리 |
| 인간 검토 생략 | 테스트만으로 부족 | 중요 코드 경로 항상 검토 |

---

## 5. Anthropic 공식 워크플로우

> 출처: [Claude Code Best Practices](https://www.anthropic.com/engineering/claude-code-best-practices)

### 탐색 → 계획 → 코드 → 커밋

```
Step 1: 탐색 (코딩 금지)
  → 관련 파일/이미지/URL 읽기
  → 복잡하면 서브에이전트 활용

Step 2: 계획 수립
  → "think", "think hard", "ultrathink"로 사고 깊이 조절
  → 계획 문서화 후 검토

Step 3: 구현
  → 단계적으로 검증하며 진행

Step 4: 커밋 & PR
```

> **핵심**: Step 1-2 없이는 Claude가 즉시 코딩 → 사전 탐색이 성능 향상의 열쇠

### 헤드리스 모드 (자동화)
```bash
claude -p "프롬프트" --output-format stream-json
```

**사용 사례:**
- 이슈 자동 분류 (GitHub 이벤트 트리거)
- 주관적 코드 리뷰 (오타, 구식 주석 감지)
- 팬아웃 패턴 (2,000개 파일 마이그레이션)

---

## 6. 에이전틱 패턴 (Agentic Patterns)

> 출처: [Awesome Agentic Patterns](https://github.com/nibzard/awesome-agentic-patterns) (2.8K stars)
> 웹사이트: [agentic-patterns.com](https://agentic-patterns.com)
> 참고: [500-AI-Agents-Projects](https://github.com/ashishpatel26/500-AI-Agents-Projects) - 500+ 에이전트 프로젝트 모음

프로덕션 환경에서 검증된 **8개 카테고리, 130+ 패턴** 모음.

| 카테고리 | 패턴 수 | 핵심 패턴 |
|----------|---------|-----------|
| **Context & Memory** | 13 | Curated Context Window, Episodic Memory |
| **Feedback Loops** | 13 | CI Feedback, Self-Critique Loop |
| **Learning** | 5 | Agent RFT, Skill Library Evolution |
| **Orchestration** | 34 | Plan-Then-Execute, Sub-Agent Spawning |
| **Reliability** | 14 | Schema Validation Retry, CriticGPT Review |
| **Security** | 3 | Isolated VM, PII Tokenization |
| **Tool Use** | 24 | Code-Over-API, CLI-First Design |
| **UX** | 13 | Human-in-the-Loop, Spectrum of Control |

### 핵심 인사이트
- **Rich Feedback > Perfect Prompts**: 완벽한 프롬프트보다 풍부한 피드백이 효과적
- **상태 외부화**: 에이전트 상태를 파일/DB에 저장 → 장기 실행 가능
- **Code-Over-API**: API 대신 코드 기반 인터페이스가 LLM 친화적

---

## 7. 검증 피드백 루프

> Boris Cherny (Claude Code 창시자): "Claude에게 작업 검증 방법을 제공하면 최종 결과 품질이 2-3배 향상"

### 도메인별 검증 방법

| 도메인 | 검증 방법 |
|--------|-----------|
| 백엔드 | 테스트 스위트 실행 |
| 프론트엔드 | 브라우저에서 확인 |
| 모바일 | 시뮬레이터 테스트 |
| CLI | bash 명령어 실행 |

### CLAUDE.md 예시
```markdown
## 검증
- 모든 변경 후 `npm test` 실행
- UI 변경 시 `npm run dev`로 브라우저 확인
- API 변경 시 `curl` 테스트
```

---

## 다음 문서

- [04-claude-code.md](04-claude-code.md) - Claude Code 상세
- [05-parallel.md](05-parallel.md) - 병렬 작업 & 오케스트레이션
