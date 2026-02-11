# 병렬 작업 & 오케스트레이션

---

## 1. 병렬 작업 방법

### Git Worktree 활용

여러 기능을 동시에 개발할 때 가장 안정적인 방법.

```bash
# 메인에서 기능별 worktree 생성
git worktree add ../feature-auth feature/auth
git worktree add ../feature-api feature/api

# 각 worktree에서 독립적으로 Claude 실행
cd ../feature-auth && claude
cd ../feature-api && claude
```

### Claude Code 백그라운드 에이전트

```
> /parallel "테스트 작성" "문서 업데이트" "린팅 수정"
```

### Boris Cherny의 병렬 워크플로우

> 5개 병렬 세션 + claude.ai에서 5-10개 추가

```bash
# 터미널 탭 1-5: 동일 저장소의 독립적인 git checkout
# 시스템 알림으로 입력 필요 시점 감지
```

---

## 2. 오케스트레이터 도구

### 스킬/워크플로우 프레임워크

| 도구 | Stars | 특징 |
|------|-------|------|
| **[Superpowers](https://github.com/obra/superpowers)** | 45K+ | TDD 강제, 5분 단위 계획, 4단계 디버깅 |
| **[SuperClaude](https://github.com/SuperClaude-Org/SuperClaude_Framework)** | 20K+ | 9개 페르소나, 19개 명령어 |

#### Superpowers
- **철학**: "계획 먼저, 코딩 나중" - 테스트 없이 코드 작성 시 삭제
- **워크플로우**: 브레인스토밍 → Git Worktree → 5분 단위 계획 → 서브에이전트 → TDD → 코드 리뷰
- **핵심 스킬**: TDD (RED-GREEN-REFACTOR), 4단계 체계적 디버깅
- **지원**: Claude Code 플러그인 마켓, Codex, OpenCode

### 도구 비교

| 도구 | Stars | 특징 |
|------|-------|------|
| **[oh-my-claude-sisyphus](https://github.com/Yeachan-Heo/oh-my-claude-sisyphus)** | 활발 | 모델 라우팅, 자동 반복 |
| **[oh-my-opencode](https://github.com/code-yeongyu/oh-my-opencode)** | 활발 | Sisyphus + OpenCode |
| **[moai-adk](https://github.com/modu-ai/moai-adk)** | 활발 | SPEC-First DDD, 한국어 |
| **[Claude-Flow](https://github.com/ruvnet/claude-flow)** | 12K+ | 멀티 에이전트 스웜 |
| **[Claude-Squad](https://github.com/smtg-ai/claude-squad)** | 5.6K | 여러 에이전트 동시 관리 |
| **[Conductor](https://conductor.build)** | - | macOS 앱, Claude Code+Codex 병렬, Git Worktree 자동화 |
| **[Amp Code](https://ampcode.com)** | - | 멀티모델 자동 라우팅 (Claude+GPT+Gemini), 서브에이전트 병렬 |

---

## 3. oh-my-claude-sisyphus

"작업 완료까지 계속 굴리는" 에이전트.

### 설치
```bash
npm install -g oh-my-claude-sisyphus
```

### 구조

```
Sisyphus (Opus) - 오케스트레이터
복잡도 분석 & 라우팅
        |
   +---------+---------+
   v         v         v
 Haiku    Sonnet     Opus
간단작업  중간작업   복잡작업
```

### 특징
- 19개 에이전트
- 지능형 모델 라우팅 (Opus→Sonnet→Haiku)
- 18개 훅
- 자동 재시도

### 현실
- 파워유저용
- 학습 곡선 있음
- 비용 최적화에 효과적

---

## 4. moai-adk

SPEC-First DDD 방법론 기반 오케스트레이터.

### 설치
```bash
# Claude Code에서 직접 사용
```

### 워크플로우
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

### 특징
- 20개 에이전트 + 49개 스킬
- 한국어 지원
- ANALYZE-PRESERVE-IMPROVE 원칙
- 재작업 90% 감소 주장

---

## 5. Claude-Flow

멀티 에이전트 스웜 플랫폼.

### 설치
```bash
npm install -g claude-flow
claude-flow init
```

### 특징
- 54+ 전문 에이전트 배포
- 분산 스웜 인텔리전스
- RAG 통합
- 공유 메모리 & 합의 시스템
- 500K+ 다운로드

### 주의사항
- GitHub Issue #653: MCP 도구의 85%가 stub/mock 구현으로 보고됨
- v3 알파 단계 (2026-02 기준), 프로덕션 사용 비권장
- 검증된 프로덕션 사용 사례 부족
- Claude Code의 네이티브 Agent Teams (Opus 4.6)이 유사 기능 제공

---

## 6. Claude-Squad

여러 AI 에이전트 동시 관리.

### 설치
```bash
brew install claude-squad
cs  # 실행
```

### 지원 에이전트
- Claude Code
- Aider
- Codex
- OpenCode
- Amp

### 특징
- tmux 기반 격리된 터미널 세션
- git worktree로 브랜치 분리
- 백그라운드 작업 (yolo/auto-accept 모드)

### 한계
- 자동 작업 분배 없음 (사용자가 수동으로 각 에이전트에 작업 할당)
- 에이전트 간 컨텍스트 공유 없음 (격리된 git 브랜치)
- 본질적으로 tmux 기반 멀티 터미널 관리 도구

---

## 7. Conductor (conductor.build)

macOS용 멀티 에이전트 오케스트레이터.

> [conductor.build](https://conductor.build)

### 특징
- Claude Code + Codex 에이전트 병렬 실행
- Git Worktree 자동 관리
- 실시간 진행 상황 대시보드
- 코드 리뷰 & 머지 UI
- 기존 Claude Code 인증 사용 (API 키 또는 Pro/Max 구독)

### Gemini CLI "Conductor"와 다름
| 구분 | Gemini CLI Conductor | conductor.build |
|------|---------------------|-----------------|
| 유형 | Gemini CLI 내장 기능 | macOS 독립 앱 |
| 용도 | 컨텍스트 파일 관리 | 멀티 에이전트 오케스트레이션 |

---

## 8. vibe-tools

### 설치
```bash
npm install -g vibe-tools
```

### 특징
- Cursor/Claude Code/Codex 지원
- Gemini 2.5 통합
- 크로스 도구 워크플로우

---

## 9. 멀티 모델 접근법 비교

| 방법 | 자동 분배 | 멀티 모델 | 설정 난이도 | 실사용 검증 |
|------|-----------|-----------|-------------|-------------|
| **Amp Code** | O (서브에이전트) | O (Claude+GPT+Gemini) | 낮음 | 검증됨 |
| **Claude Code Agent Teams** | O (Opus 4.6 내장) | X (Claude만) | 없음 | 검증됨 |
| **OpenRouter + Claude Code** | X | O (400+ 모델) | 낮음 | 검증됨 |
| **OpenCode** | X (수동) | O (75+ 제공자) | 중간 | 검증됨 |
| **Claude Squad** | X (수동) | O (다른 에이전트 실행) | 낮음 | 검증됨 |
| **Claude-Flow** | O (이론상) | O (이론상) | 높음 | 미검증 |

### OpenRouter 활용법

Claude Code에서 [OpenRouter](https://openrouter.ai)를 경유하면 400+ 모델 전환이 가능.

```bash
export ANTHROPIC_BASE_URL=https://openrouter.ai/api/v1
export ANTHROPIC_API_KEY=sk-or-...
claude
/model deepseek/deepseek-chat    # 간단한 작업 (저렴)
/model anthropic/claude-opus-4   # 복잡한 작업 (고성능)
```

**주의**: 코드가 OpenRouter 서버를 경유하므로 프라이버시 고려 필요.

---

## 10. 실용적 팁

### 언제 병렬 작업을 사용할까?

| 상황 | 권장 |
|------|------|
| 독립적인 기능 2-3개 | Git Worktree |
| 단순 반복 작업 | /parallel |
| 복잡한 시스템 변경 | 순차 작업 권장 |
| 팀 협업 | Claude-Squad |

### 비용 고려

| 방식 | 비용 영향 |
|------|-----------|
| 순차 작업 | 기본 |
| 병렬 2-3개 | 2-3배 |
| 오케스트레이터 | 모델 라우팅으로 최적화 가능 |

### 주의사항
- 병렬 작업 간 충돌 주의
- 같은 파일 동시 수정 피하기
- 정기적으로 동기화

---

## 다음 문서

- [06-practices.md](06-practices.md) - 실전 기법 & 패턴
- [references.md](references.md) - 참고 자료

---

*마지막 업데이트: 2026-02-10*
