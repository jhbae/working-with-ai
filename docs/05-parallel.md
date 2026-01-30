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

### 도구 비교

| 도구 | Stars | 특징 |
|------|-------|------|
| **[oh-my-claude-sisyphus](https://github.com/Yeachan-Heo/oh-my-claude-sisyphus)** | 활발 | 모델 라우팅, 자동 반복 |
| **[oh-my-opencode](https://github.com/code-yeongyu/oh-my-opencode)** | 활발 | Sisyphus + OpenCode |
| **[moai-adk](https://github.com/modu-ai/moai-adk)** | 활발 | SPEC-First DDD, 한국어 |
| **[Claude-Flow](https://github.com/ruvnet/claude-flow)** | 12K+ | 멀티 에이전트 스웜 |
| **[Claude-Squad](https://github.com/smtg-ai/claude-squad)** | 5.6K | 여러 에이전트 동시 관리 |
| **[Conductor](https://conductor.build)** | - | macOS 앱, Claude Code+Codex 병렬, Git Worktree 자동화 |

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

## 9. 실용적 팁

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

*마지막 업데이트: 2026-01-30*
