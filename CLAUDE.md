# AI Development Guide Project

## Project Overview
AI 기반 개발 도구와 방법론에 대한 종합 가이드 문서 프로젝트

## Language
- 문서는 한국어로 작성
- 기술 용어는 영어 병기 권장 (예: "스펙 기반 개발(Spec-Driven Development)")

## Writing Style
- 실용적이고 간결한 문체
- 과장 없이 객관적 정보 제공
- 실제 채택률과 커뮤니티 피드백 기반 평가
- 도구별 장단점 균형있게 서술

## File Structure
```
ai-dev-guide/
├── README.md              # 프로젝트 소개
├── CLAUDE.md              # 프로젝트 설정
├── docs/
│   ├── README.md          # 빠른 시작 & 권장사항
│   ├── 01-overview.md     # 현황, 채택률, 분류, 2026 트렌드
│   ├── 02-tools.md        # 도구 종합 비교
│   ├── 03-methodology.md  # 개발 방법론
│   ├── 04-claude-code.md  # Claude Code 완전 가이드
│   ├── 05-parallel.md     # 병렬 작업 & 오케스트레이션
│   ├── 06-practices.md    # 실전 기법 & 패턴
│   └── references.md      # 참고 자료 & 영향력자
└── examples/              # 예제 설정 파일
```

## Key Commands
- Build: N/A (documentation project)
- Test: `markdownlint docs/`

## Important Notes
- 도구 평가 시 GitHub stars, 실제 사용 후기, 채택률 데이터 참고
- 과대 광고성 내용 배제, 실제 효과 중심 서술
- 2025-2026년 최신 정보 기준

## Documentation Rules

### 문서 구조
- 모든 문서: `# 제목` → `> 메타 주석` → 본문 → `## 다음 문서` → `*업데이트 날짜*`
- 200줄 이상 문서: 목차 필수

### 날짜/버전 표기
- 업데이트 날짜: `*마지막 업데이트: YYYY-MM-DD*` (문서 끝)
- 도구 버전: `(YYYY-MM-DD 기준)` 인라인
- 통계: 반드시 출처 포함 `(출처: [이름](URL))`

### Cross-Reference (상호 참조)
references.md 업데이트 시 관련 본문 문서도 함께 수정:

| references.md 추가 | 본문 반영 위치 |
|-------------------|---------------|
| 영향력자/인물 | 06-practices.md "인사이트" 또는 references.md "영향력자" |
| 도구 | 02-tools.md 해당 카테고리 테이블 |
| 커뮤니티 프로젝트 | 05-parallel.md 또는 04-claude-code.md |
| 참고 자료 | 해당 주제 문서에 인용/링크 |

### 인용 스타일
```markdown
# 인물 인용
> "인용문"
> — 인물명, [출처](URL)

# 통계
87% 개발자가 사용 (출처: [Stack Overflow 2025](URL))
```

### 금지 사항
- 이모지 사용 금지 (일관성)
- 출처 없는 통계 금지
- 날짜 표기 누락 금지
