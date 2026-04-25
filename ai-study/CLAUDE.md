# 프로젝트2 — AI 영어 학습 모드

## 프로젝트 설명
Anthropic Claude API를 호출해서 영어 학습을 돕는 단일 HTML 앱(`A.html`).
세 가지 모드로 동작:
1. **문법 분석** — 영문장의 문장 구조(주어/동사/목적어/수식어), 문법 포인트, 품사 분해
2. **표현 확장** — 표현의 유사 표현, 어원, 사용 팁
3. **퀴즈** — 빈칸/변형/오류수정/번역 4종. 약점 기반 자동 출제 지원

핵심: 사용자 상태(레벨, 약점, 정답률, 토픽별 점수)를 localStorage에 누적 저장하고,
오답이 2회 이상인 토픽은 자동으로 약점 등록, 3연속 정답이면 자동 해제 — 적응형 학습 루프.

## 기술 스택
- **단일 파일**: `A.html` (HTML + CSS + Vanilla JS, 빌드 도구 없음)
- **API**: Anthropic Messages API (모델: `claude-haiku-4-5-20251001`)
  - 브라우저 직접 호출 (`anthropic-dangerous-direct-browser-access: true`)
- **저장소**: localStorage (사용자 상태/히스토리), sessionStorage (API 키 옵션)
- **외부 의존**: Google Fonts (Noto Sans KR, JetBrains Mono)

## 주의사항

### 보안
- API 키는 브라우저에 저장됨. base64 인코딩은 어깨너머 가림용일 뿐 보안 아님.
- `dangerously-allow-direct-browser-access` 헤더 사용 — 키가 클라이언트에서 노출됨.
- **공유/배포 시 반드시 백엔드 프록시 필요** (Cloudflare Workers, Vercel Edge Function 등).
- 현재는 본인 PC 학습 도구 가정. 공용 PC는 "세션 저장" 옵션 권장.

### 코드 수정 시
- CSS는 섹션별로 정리되어 있음 (1. Tokens → 22. Animations). 새 스타일 추가 시 해당 섹션 안에.
- JS는 모듈 패턴 없이 IIFE + 함수 분리. 전역 `AI` 객체에 상태가 모임.
- minify 금지 — 가독성 우선.
- 디자인 토큰(`--primary`, `--surface` 등)을 직접 hex로 하드코딩하지 말 것.

### AI 응답 파싱
- Claude가 JSON 외 텍스트도 같이 반환하는 경우가 있어, 정규식 `\{[\s\S]*\}`로 추출 후 `JSON.parse`.
- 모델 변경 시 `apiCall`의 `model` 필드만 수정하면 됨.

### 약점 자동 감지 로직
- `autoDetectWeakness`: 최근 30개 퀴즈 중 토픽별 2회 이상 오답 → 약점 등록 (최대 5개)
- `updateQuizStats`: 토픽 3연속 정답 → 약점 자동 해제
- 수동 약점은 자동 감지된 약점과 합쳐 최대 10개까지 유지

### 접근성
- 모달은 `role="dialog" aria-modal="true"`, ESC로 닫힘.
- 모드/레벨 버튼은 `role="radiogroup"` + `aria-checked`.
- `prefers-reduced-motion` 존중.
