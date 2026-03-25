# 실전 기법 & 패턴

AI 에이전트 활용 시 검증된 기법과 패턴 모음.

---

## 1. 좋은 스펙 작성법 (Addy Osmani)

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

### Addy Osmani의 2026 LLM 워크플로우 업데이트

> 출처: [My LLM Coding Workflow Going Into 2026](https://addyosmani.com/blog/ai-coding-workflow/)

**핵심 변경점:**
- 계획 우선(Planning-First): 강건한 스펙이 모든 작업의 초석
- LLM은 방향 제시가 필요한 강력한 페어 프로그래머
- 품질 게이트 강화: 더 많은 테스트, 모니터링, AI-on-AI 코드 리뷰

**Context Engineering 강조:**
- 프롬프트 엔지니어링에서 컨텍스트 엔지니어링으로 전환
- CLAUDE.md/AGENTS.md가 핵심 컨텍스트 도구
- 선택, 압축, 순서, 격리, 형식 최적화 5대 전략

---

## 2. Anthropic 공식 워크플로우

> 출처: [Claude Code Best Practices](https://www.anthropic.com/engineering/claude-code-best-practices)

### 탐색 → 계획 → 코드 → 커밋

```
Step 1: 탐색 (코딩 금지)
  → 관련 파일/이미지/URL 읽기
  → 복잡하면 서브에이전트 활용

Step 2: 계획 수립
  → Thinking mode 활용 (TAB으로 토글, 기본 활성화)
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

## 3. 에이전틱 패턴 (Agentic Patterns)

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

## 4. 검증 피드백 루프

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

### AI 코드 추적 주석

AI 생성 코드의 리뷰 상태와 보안 위험을 명시적으로 표시하는 기법.

```typescript
// AI-REVIEWED: false — 아직 인간 리뷰 미완료
function processUserInput(data: string) { ... }

// AI-REVIEWED: true — 2026-02-10 검토 완료
function validateEmail(email: string) { ... }

// SECURITY-CRITICAL: 인증 로직, AI 수정 금지
// AI-REVIEWED: true
function verifyJWT(token: string) { ... }
```

| 주석 | 용도 |
|------|------|
| `AI-REVIEWED: false` | AI 생성 후 인간 리뷰 대기 |
| `AI-REVIEWED: true` | 인간 검토 완료 |
| `SECURITY-CRITICAL` | AI가 수정하면 안 되는 민감 영역 |

> 출처: [How to Effectively Write Quality Code with AI](https://heidenstedt.org/posts/2026/how-to-effectively-write-quality-code-with-ai/)

---

## 5. Ralph Loop (자율 반복 패턴)

> "Ralph는 Bash 루프다" - Geoffrey Huntley
> 이름 유래: 심슨의 Ralph Wiggum - 항상 실수하지만 절대 멈추지 않는다

**참고**: Ralph Loop은 **패턴/기법**이고, [Sisyphus](05-parallel.md#4-oh-my-claude-sisyphus)는 이 패턴을 포함한 **풀 오케스트레이터**입니다.

| Ralph Loop | Sisyphus |
|-----------|----------|
| 단순 반복 패턴 | 멀티 에이전트 오케스트레이터 |
| 단일 에이전트 루프 | 모델 라우팅 (Opus→Sonnet→Haiku) |
| bash while 수준 | LSP, AST, 병렬 태스크 활용 |

### 핵심 개념

PRD의 모든 항목이 완료될 때까지 AI 에이전트를 **반복 실행**하는 패턴.

```
while (not complete) {
  1. Fresh context로 에이전트 시작
  2. 작업 시도
  3. 완료 태그 체크 (<promise>COMPLETE</promise>)
  4. 미완료 시 → 피드백과 함께 재시작
}
```

### 왜 Fresh Context인가?

| 문제 | Ralph 해결책 |
|------|-------------|
| 컨텍스트 누적 오염 | 매 반복 fresh start |
| 실패 히스토리 노이즈 | git이 memory layer |
| 컨텍스트 윈도우 한계 | 작은 단위 PRD |

### 구현 방식

```bash
# 기본 Ralph Loop (개념)
while true; do
  claude -p "PRD.md 기반으로 작업 진행" --output-format json

  # 완료 체크
  if grep -q "COMPLETE" output.json; then
    break
  fi

  # 피드백 수집 후 재시작
  git diff > feedback.txt
done
```

### PRD 구조 예시
```markdown
# PRD: 사용자 인증 기능

## Stories
- [ ] 로그인 폼 구현
- [ ] JWT 토큰 발급
- [ ] 세션 관리

## 완료 조건
- 모든 테스트 통과
- 린트 에러 없음
```

### 실전 팁

| 팁 | 설명 |
|----|------|
| **작은 PRD** | 한 컨텍스트 윈도우에서 완료 가능한 크기 |
| **AGENTS.md 활용** | 매 반복 후 학습 내용 기록 → 다음 반복에 반영 |
| **테스트 주도** | 완료 조건을 테스트로 명확하게 정의 |
| **git이 메모리** | 상태는 파일로, 히스토리는 git으로 |

### 구현체 & 참고자료

| 리소스 | 설명 |
|--------|------|
| [ghuntley.com/loop](https://ghuntley.com/loop/) | Geoffrey Huntley 원본 글 |
| [ralph-playbook](https://github.com/ClaytonFarr/ralph-playbook) | 종합 가이드 |
| [snarktank/ralph](https://github.com/snarktank/ralph) | PRD 기반 자율 루프 |
| [vercel-labs/ralph-loop-agent](https://github.com/vercel-labs/ralph-loop-agent) | AI SDK 호환 구현 |
| [Goose Ralph Loop 튜토리얼](https://block.github.io/goose/docs/tutorials/ralph-loop/) | Goose에서 Ralph 사용법 |
| [Awesome Claude - Ralph](https://awesomeclaude.ai/ralph-wiggum) | Claude Code용 가이드 |

### 실제 사례

> Cursor CLI + Replica MCP로 Fruit Ninja 클론 제작
> - 8번의 컨텍스트 로테이션
> - 캔버스 렌더링 여러 번 실패 후 학습
> - ~1시간 만에 완성, 인간 개입 0

---

## 6. 에이전트 오케스트레이션 전환 (2026)

> 출처: [Anthropic 2026 Agentic Coding Trends Report](https://resources.anthropic.com/hubfs/2026%20Agentic%20Coding%20Trends%20Report.pdf)

### 핵심 발견

| 지표 | 수치 |
|------|------|
| AI 활용 비율 | 업무의 ~60% |
| 완전 위임 가능 작업 | 0-20% |
| Rakuten 사례 | 1,250만줄 코드베이스, 99.9% 정확도, 자율 7시간 |
| AI 일일 사용률 | 73% (2024년 18%에서 급증) |
| GitHub 커밋 AI 지원 | 51% (2026 초) |
| AI 코드 버그 밀도 | 인간 코드 대비 1.7배 |
| AI 코드 보안 취약점 | 45% 포함, 인간 대비 2.74배 |

### 역할 변화

> "엔지니어는 코드를 작성하는 것에서 코드를 작성하는 에이전트를 조율하는 것으로 전환하고 있다. 자신의 전문성은 아키텍처, 시스템 설계, 전략적 결정에 집중."

### Guy Podjarny (Tessl 창업자)의 전망
> "2027년 말이면, 에이전트와 일하는 개발자는 대부분의 시간 동안 코드를 보지 않을 것이다."
> — Guy Podjarny, [AI Native Dev](https://ainativedev.io)

---

## 7. Mitchell Hashimoto의 AI 도입 6단계

> 출처: [My AI Adoption Journey](https://mitchellh.com/writing/my-ai-adoption-journey)
> Mitchell Hashimoto - HashiCorp 창립자, Ghostty 개발자

### 6단계 프레임워크

| 단계 | 이름 | 설명 |
|------|------|------|
| 1 | **챗봇 버리기** | 웹 챗 인터페이스 대신 에이전트(파일 읽기, 프로그램 실행 가능) 사용 |
| 2 | **직접 재현** | 수동으로 한 번, 에이전트로 한 번 - 같은 작업을 두 번 수행하며 학습 |
| 3 | **퇴근 전 30분** | 하루 끝 30분간 에이전트에 리서치/이슈 분류 위임 |
| 4 | **확실한 것만 위임** | 확신 있는 작업만 백그라운드 에이전트에 맡기고, 깊은 작업에 집중 |
| 5 | **하네스 엔지니어링** | AGENTS.md로 반복 실수 방지, 검증 스크립트 구축 |
| 6 | **항상 에이전트 실행** | 대기열 작업에 에이전트 상시 실행, 근무 시간의 10-20% 커버 |

### 핵심 원칙

**"Don't Draw the Owl"**
> "하나의 메가 세션으로 그림을 완성하려 하지 마라. 명확하고 검증 가능한 작업 단위로 분해하라."

**하네스 엔지니어링**
- AGENTS.md에 에이전트가 반복하는 실수를 기록
- 커스텀 검증 스크립트 (스크린샷, 필터된 테스트)
- 에이전트 알림 끄기 - 컨텍스트 스위칭 비용 방지

> "가치를 찾으려면 에이전트를 사용해야 한다. 초기 마찰을 견뎌야 돌파구가 온다."

---

## 8. Compound Engineering (복리형 개발)

> 출처: [Compound Engineering](https://every.to/guides/compound-engineering)
> Kieran Klaassen - Cora (AI 이메일 비서) 개발자

### 핵심 개념

> "모든 엔지니어링 작업이 다음 작업을 더 쉽게 만들어야 한다."

기존 개발에서는 코드베이스가 커지면 복잡도가 누적되지만, Compound Engineering에서는 버그 수정, 기능 추가, 패턴 발견을 **학습 가능한 자산**으로 전환한다.

### 4단계 루프

```
Plan → Work → Review → Compound → (반복)
```

| 단계 | 설명 | 시간 비율 |
|------|------|-----------|
| **Plan** | 요구사항 분석, 코드베이스 조사, 설계 | 40% |
| **Work** | AI 에이전트가 구현 | 10% |
| **Review** | 14+ 전문 에이전트가 병렬 검토 | 40% |
| **Compound** | 패턴을 시스템에 인코딩, 미래 작업 자동화 | 10% |

> **80%가 계획과 리뷰, 20%가 실행** - 기존 개발의 역전

### 리뷰 에이전트 예시

| 역할 | 에이전트 | 검토 대상 |
|------|----------|-----------|
| 보안 | security-sentinel | OWASP Top 10 |
| 성능 | performance-oracle | N+1 쿼리, 캐싱 |
| 아키텍처 | architecture-strategist | 설계 패턴 |
| 데이터 | data-integrity-guardian | 데이터 무결성 |
| 코드 품질 | code-simplicity-reviewer | 단순성 |

### 개발자 성장 5단계

| 단계 | 방식 | 설명 |
|------|------|------|
| Stage 0 | 수동 개발 | AI 미사용 |
| Stage 1 | 챗 보조 | 코드 조각 복사 |
| Stage 2 | 에이전트 + 라인별 리뷰 | 한 줄씩 검토 |
| **Stage 3** | **계획 우선, PR 리뷰** | **Compound Engineering 시작** |
| Stage 4 | 아이디어→PR 자동화 | 단일 머신 |
| Stage 5 | 클라우드 병렬 실행 | 여러 기능 동시 |

### 버려야 할 8가지 믿음

1. "코드는 손으로 작성해야 한다" → 품질이 중요, 누가 타이핑하느냐는 아님
2. "모든 라인을 수동 리뷰해야 한다" → 자동화된 리뷰로 대체 가능
3. "솔루션은 엔지니어에게서만" → AI가 리서치하고 인간이 판단
4. "코드가 주요 산출물이다" → **계획이 새로운 코드**
5. "코드 작성이 핵심 업무" → 가치 전달이 핵심, 코드는 수단
6. "첫 시도가 좋아야 한다" → 95% 쓰레기 예상, 빠른 반복
7. "코드는 자기 표현이다" → 팀과 사용자의 것
8. "타이핑이 학습이다" → 리뷰를 통한 이해가 더 중요

> "산출물은 키 입력 횟수가 아니라 해결한 문제로 측정해야 한다."

---

## 9. AI 코드 품질 & 보안 (2026)

> 2025년이 속도의 해였다면, 2026년은 품질의 해.
> 출처: [CodeRabbit](https://www.coderabbit.ai/blog/2025-was-the-year-of-ai-speed-2026-will-be-the-year-of-ai-quality)

### 코드 품질 현실

| 지표 | 수치 | 출처 |
|------|------|------|
| AI 코드 버그 밀도 | 인간 대비 1.7배 | Second Talent 2026 |
| 로직/정확성 이슈 | 75% 더 많음 (다운스트림 인시던트 기여) | Second Talent 2026 |
| 수동 리뷰 없이 머지 거부 | 71% | Panto AI Statistics |
| AI 코드 "높은 신뢰" | 3%만 | AI Coding Statistics |
| 테스트 커버리지 허상 | AI가 95% 커버리지 생성 가능하나 실제 버그 포착률 30-40% | Mutation Testing 연구 |

### 보안 취약점 현황

| 지표 | 수치 | 출처 |
|------|------|------|
| AI 코드 보안 취약점 포함 | 45% | [Veracode](https://www.veracode.com/resources/analyst-reports/2025-genai-code-security-report/) |
| 인간 코드 대비 취약점 | 2.74배 | Veracode |
| XSS 방어 실패 | 86% (CWE-80) | CSA |
| 로그 인젝션 취약 | 88% (CWE-117) | CSA |
| Java 실패율 | 70% | Veracode |
| Python/C#/JS 실패율 | 38-45% | Veracode |
| 패스워드 처리 이슈 | 1.88배 더 많음 | Veracode |

### 품질 보장 전략

| 전략 | 설명 |
|------|------|
| **변이 테스트 (Mutation Testing)** | 코드 커버리지 대신 실제 결함 검출 능력 측정 |
| **AI-on-AI 리뷰** | Devin Review 등 AI가 AI 코드를 리뷰 |
| **결함 밀도 추적** | 단순 버그 수 대신 심각도 기반 밀도 |
| **인시던트 상관관계** | AI 코드 영역과 프로덕션 인시던트 상관관계 분석 |
| **SAST + AI** | 정적 분석 도구와 AI 리뷰 병행 |

### METR 생산성 패러독스

> 출처: [METR Study](https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/)

숙련 오픈소스 개발자 대상 연구에서, AI 도구 사용 시:
- 개발자 인식: "20% 빨라졌다"
- 실제 측정: **19% 느려짐**
- 원인: 프롬프트 작성, 결과 검증, 컨텍스트 전환 오버헤드

**시사점**: 생산성 향상은 작업 유형과 개발자 경험에 따라 크게 다름. 일률적 적용은 위험.

---

## 다음 문서

- [references.md](references.md) - 참고 자료

---

*마지막 업데이트: 2026-03-25*
