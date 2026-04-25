# DESIGN.md — 웹 UI/UX 가이드

## 1. 색상 시스템

CSS 변수로 정의해 사용합니다.

```css
:root {
  /* Primary */
  --color-primary: #2563eb;
  --color-primary-hover: #1d4ed8;
  --color-primary-light: #dbeafe;

  /* Neutral */
  --color-text: #0f172a;
  --color-text-muted: #64748b;
  --color-border: #e2e8f0;
  --color-bg: #ffffff;
  --color-bg-alt: #f8fafc;

  /* Semantic */
  --color-success: #16a34a;
  --color-warning: #ea580c;
  --color-error: #dc2626;
  --color-info: #0284c7;
}
```

다크모드 지원 시 `[data-theme="dark"]` 또는 `prefers-color-scheme` 활용.

---

## 2. 타이포그래피

```css
:root {
  --font-sans: -apple-system, BlinkMacSystemFont, 'Pretendard', 'Segoe UI', sans-serif;
  --font-mono: 'JetBrains Mono', 'D2Coding', monospace;

  --text-xs: 0.75rem;    /* 12px */
  --text-sm: 0.875rem;   /* 14px */
  --text-base: 1rem;     /* 16px */
  --text-lg: 1.125rem;   /* 18px */
  --text-xl: 1.25rem;    /* 20px */
  --text-2xl: 1.5rem;    /* 24px */
  --text-3xl: 1.875rem;  /* 30px */

  --leading-tight: 1.25;
  --leading-normal: 1.5;
  --leading-relaxed: 1.75;
}
```

본문: `--text-base` + `--leading-normal`
제목: `--text-2xl` 이상 + `--leading-tight`

---

## 3. 간격 (Spacing)

8px 그리드 기반:

```css
:root {
  --space-1: 0.25rem;  /* 4px */
  --space-2: 0.5rem;   /* 8px */
  --space-3: 0.75rem;  /* 12px */
  --space-4: 1rem;     /* 16px */
  --space-6: 1.5rem;   /* 24px */
  --space-8: 2rem;     /* 32px */
  --space-12: 3rem;    /* 48px */
  --space-16: 4rem;    /* 64px */
}
```

---

## 4. 반응형 브레이크포인트

```css
/* 모바일 우선 */
/* 기본: ~639px (모바일) */

@media (min-width: 640px)  { /* sm: 태블릿 세로 */ }
@media (min-width: 768px)  { /* md: 태블릿 가로 */ }
@media (min-width: 1024px) { /* lg: 데스크톱 */ }
@media (min-width: 1280px) { /* xl: 와이드 */ }
```

---

## 5. 컴포넌트 네이밍

BEM 또는 단순 kebab-case:

| 종류 | 패턴 | 예 |
|---|---|---|
| 블록 | `block-name` | `card`, `nav-bar` |
| 변형 | `block-variant` | `btn-primary`, `card-highlight` |
| 상태 | `is-state`, `has-state` | `is-active`, `is-loading` |
| 자식 요소 | `block__element` | `card__title`, `nav-bar__item` |

---

## 6. 버튼 스타일 표준

```css
.btn {
  padding: var(--space-2) var(--space-4);
  border-radius: 6px;
  font-size: var(--text-base);
  font-weight: 500;
  cursor: pointer;
  transition: background 150ms;
}

.btn-primary { background: var(--color-primary); color: white; }
.btn-primary:hover { background: var(--color-primary-hover); }

.btn-secondary { background: transparent; color: var(--color-primary); border: 1px solid var(--color-primary); }
```

---

## 7. 접근성 (필수)

- 모든 인터랙티브 요소: 키보드 포커스 가능
- 포커스 링: `outline: 2px solid var(--color-primary)` 유지
- 색만으로 정보 전달 금지 (아이콘·텍스트 병행)
- 색 대비: 본문 4.5:1 이상, 큰 글자 3:1 이상 (WCAG AA)
- `prefers-reduced-motion` 존중 (애니메이션 줄이기)
- 폼 필드: `<label>` 필수 + 에러 메시지 `aria-describedby` 연결

---

## 8. 레이아웃 원칙

- 컨테이너 최대 너비: 1200px (`max-width: 1200px; margin: 0 auto`)
- 모바일 패딩: `--space-4`, 데스크톱: `--space-8`
- Flex/Grid 우선 (float 금지)
- 수직 정렬은 Flexbox `align-items: center`

---

## 9. 애니메이션·전환

- 전환 시간: 150ms (빠른 피드백) ~ 300ms (부드러운 전환)
- Easing: `ease-out` (들어옴), `ease-in` (나감), `ease-in-out` (양방향)
- 60fps 보장 (`transform`, `opacity` 만 애니메이션, `width`/`height` 금지)

---

## 10. 아이콘·이미지

- 아이콘: SVG 우선 (Lucide, Heroicons 권장)
- 사진: WebP 우선, fallback으로 JPG
- 모든 이미지 `loading="lazy"` (above-the-fold 제외)
- `width`, `height` 속성 명시 (CLS 방지)
