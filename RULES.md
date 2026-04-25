# RULES.md — 코딩 규칙·금지 사항

## 1. 절대 금지

- `console.log`, `print()` 디버그 흔적 커밋 금지
- 주석 처리한 코드 커밋 금지 (Git 히스토리로 관리)
- 사용하지 않는 import·변수 남기지 말 것
- `var` 사용 금지 → `const` 우선, 재할당 시만 `let`
- `==` 사용 금지 → `===` 만 사용
- `eval`, `Function()` 동적 실행 금지
- `innerHTML` / `dangerouslySetInnerHTML` sanitize 없이 사용 금지
- 매직 넘버·매직 스트링 금지 → 상수로 추출
- API 키·비밀번호 하드코딩 금지 → `.env` 사용

---

## 2. 항상 할 것

- 함수·변수명은 의도가 드러나게 (축약 금지: `usr` ✗ → `user` ✓)
- 외부 API 호출은 try-catch + 사용자에게 보일 에러 메시지
- 비동기 함수는 명확히 `async` 표시 + 에러 처리
- 사용자 입력은 항상 검증·sanitize
- 함수 한 개는 한 가지 일만 (50줄 넘으면 분리 검토)
- 파일 800줄 넘으면 분리

---

## 3. 코드 수정·개선 작업 규칙

- 수정 전 영향받는 파일·함수 목록 먼저 보고
- 기존 컨벤션 유지 (새 컨벤션 도입 시 반드시 확인 요청)
- 리팩토링과 기능 변경은 **분리해서 커밋**
- 동작 변경 없는 리팩토링은 테스트 결과가 동일해야 함
- 기존 함수 시그니처 변경 시: 호출부 전체 확인 후 진행

---

## 4. 웹 개발 규칙

### HTML
- Semantic 태그 우선 (`<main>`, `<nav>`, `<section>`, `<article>`)
- `<div>` 남용 금지
- 모든 `<img>`에 `alt` 속성
- 모든 `<button>`에 명확한 텍스트 또는 `aria-label`
- 폼은 `<label>` + `for` 속성 필수

### CSS
- 클래스명: kebab-case (`hero-visual`)
- ID 선택자 스타일링 금지 (재사용성)
- `!important` 금지 (특이성 충돌 시만 예외)
- 매직 픽셀값 금지 → CSS 변수 사용 (`--spacing-md`)
- 인라인 스타일 금지 (예외: 동적 값)

### JavaScript
- 화살표 함수 우선 (메소드 정의 제외)
- 구조 분해 활용 (`const { x, y } = obj`)
- 옵셔널 체이닝 활용 (`obj?.prop?.value`)
- 배열은 불변 메소드 우선 (`map`, `filter`, `reduce`)
- DOM 접근 캐싱 (반복 `querySelector` 금지)

### 접근성 (a11y)
- 색 대비 WCAG AA 이상
- 키보드만으로 모든 기능 사용 가능해야 함
- 포커스 스타일 제거 금지

---

## 5. 코딩 스타일

### 형식
- 들여쓰기: 2칸 (Python 제외, Python은 4칸)
- 따옴표: 싱글 (`'`) 우선
- 세미콜론: 사용
- 줄 끝 공백 제거
- 파일 끝 빈 줄 1개

### 명명
- JS 변수·함수: camelCase (`getUserName`)
- CSS 클래스: kebab-case (`hero-visual`)
- CSS 변수: `--kebab-case` (`--color-primary`)
- 상수: UPPER_SNAKE_CASE
- Python 함수·변수: snake_case / 클래스: PascalCase
- 사이드 이펙트 있는 함수는 이름에 드러내기 (`fetchAndSaveUser`)

### 주석
- "왜" 그렇게 짰는지를 설명 (무엇을 하는지는 코드 자체로 드러남)
- 의미 없는 주석 금지 (`// 변수 선언`)

---

## 6. Git·커밋 규칙

- 한 커밋 = 한 가지 변경
- 커밋 메시지는 영문 또는 한글 일관되게
- 형식: `<type>: <한 줄 설명>`
- type: `feat`, `fix`, `refactor`, `docs`, `test`, `chore`, `perf`, `ci`
- `git push --force` 금지 (개인 브랜치 제외)

---

## 7. 의존성 관리

- 새 패키지 설치 전 사용자에게 한 줄 보고
- 라이브러리 선택 기준: 유지보수 활발 + 다운로드 수 + 라이선스 확인
- `package.json` / `requirements.txt` 항상 최신 상태 유지
- `node_modules`, `__pycache__`, `.env` 는 `.gitignore`
