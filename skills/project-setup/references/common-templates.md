# Common Templates

모든 프론트엔드 프로젝트에 공통 적용되는 템플릿.

---

## CLAUDE.md

```markdown
# {{PROJECT_TITLE}}

> {{DESCRIPTION}}
> 문서 목차 및 핵심 원칙: [AGENTS.md](AGENTS.md)

## 프로젝트 개요

<!-- TODO: 프로젝트 배경, 목표, 핵심 기능 서술 -->

## 핵심 플로우

\`\`\`
<!-- TODO: 핵심 사용자 플로우를 ASCII로 시각화 -->
[Step 1] → [Step 2] → [Step 3]
\`\`\`

## 기술 스택

| 영역 | 기술 |
|------|------|
| 프레임워크 | {{TECH_STACK}} |
| 스타일링 | Tailwind CSS |
| 언어 | TypeScript strict |
<!-- TODO: 추가 스택 기입 -->

## 주요 명령어

\`\`\`bash
npm run dev          # 개발 서버
npm run build        # 프로덕션 빌드
npx tsc --noEmit     # 타입 검사
\`\`\`

## 폴더 구조

\`\`\`
src/
├── app/             # 페이지 + 라우팅
├── components/
│   └── ui/          # 범용 UI 컴포넌트
└── lib/             # 유틸, 타입, 도메인 로직
    └── hooks/       # 커스텀 훅
\`\`\`
<!-- TODO: 실제 구조에 맞게 업데이트 -->

## 코드 스타일

- TypeScript `strict: true`, `any` 금지
- 컴포넌트: PascalCase / 함수, 변수: camelCase / 상수: UPPER_SNAKE_CASE
- 서버 컴포넌트 기본, 클라이언트는 `'use client'` 선언
- 주석: 한글, 복잡한 비즈니스 로직에만

## 설계 원칙

<!-- TODO: 프로젝트 고유 원칙 추가 -->

## 최근 변경사항

- {{DATE}} 프로젝트 초기 세팅

## 참조 문서

<!-- TODO: 관련 외부 문서/링크 추가 -->
- 디자인 컨텍스트: [워크스페이스 .impeccable.md](../../.impeccable.md)
```

---

## AGENTS.md

```markdown
# {{PROJECT_TITLE}} - Agent Guide

> 이 파일은 목차다. 상세 내용은 각 링크를 따라가라.

## Quick Start

1. `npm run dev` - 개발 서버
2. `npm run build` - 프로덕션 빌드
3. `npx tsc --noEmit` - 타입 검사

## Golden Principles

1. **컴포넌트 co-location**: 페이지 전용 컴포넌트는 `src/app/{feature}/_components/`에 배치
2. **공유 UI**: `src/components/ui/`에만 배치. 범용이 아니면 `_components/`에
3. **공유 상수 한 곳에**: `lib/constants.ts`에서 관리
4. **비즈니스 로직 분리**: 컴포넌트에 계산 로직 금지. `lib/{domain}.ts`에 분리
5. **UI 텍스트 한국어**, 코드(변수/함수명) 영어

## Key Directories

\`\`\`
src/
  app/              # 페이지 + 라우팅
  components/ui/    # 범용 UI 컴포넌트
  lib/              # 유틸, 타입, 도메인 로직
    hooks/          # 커스텀 훅
\`\`\`

## Docs Map

| 문서 | 용도 | 변경 빈도 |
|------|------|----------|
| [CLAUDE.md](CLAUDE.md) | 진입 계약서 (개요, 스택, 규칙, 상태) | 상시 |
| [ARCHITECTURE.md](ARCHITECTURE.md) | 의존성 레이어, 데이터 흐름 | 구조 변경 시 |
| [docs/PRODUCT_SENSE.md](docs/PRODUCT_SENSE.md) | 제품 방향, 사용자, 목표 | 월간 |
| [docs/DESIGN.md](docs/DESIGN.md) | 디자인 시스템, 색상, 타이포그래피 | 기능 추가 시 |
| [docs/FRONTEND.md](docs/FRONTEND.md) | 컴포넌트 패턴, 스타일 규칙 | 기능 추가 시 |
| [docs/PLANS.md](docs/PLANS.md) | 우선순위, 로드맵, 기술 부채 | 주간 |
| [docs/design-docs/](docs/design-docs/) | 설계 의사결정 기록 (DD-001~) | 기능 추가 시 |
| [docs/exec-plans/](docs/exec-plans/) | 실행 계획, 테스트 케이스 (EP-001~) | Phase 착수 시 |
```

---

## ARCHITECTURE.md

```markdown
# {{PROJECT_TITLE}} - Architecture

## 의존성 레이어

\`\`\`
Layer 0: Types          (lib/types.ts)
Layer 1: Constants      (lib/constants.ts)
Layer 2: Domain Logic   (lib/{domain}.ts)
Layer 3: UI Components  (components/ui/)
Layer 4: Page Components (app/{feature}/_components/)
Layer 5: Pages          (app/{feature}/page.tsx)
\`\`\`

**규칙**: 상위 레이어는 하위 레이어만 import. 역방향 금지.

## 데이터 흐름

<!-- TODO: 프로젝트 고유 데이터 흐름 서술 -->

## 모듈 경계

<!-- TODO: 주요 모듈 간 의존성 서술 -->

## 확장 포인트

<!-- TODO: 향후 확장 가능한 지점 기록 -->
```

---

## docs/PRODUCT_SENSE.md

```markdown
# {{PROJECT_TITLE}} - Product Sense

## 미션

<!-- TODO: 이 제품이 존재하는 이유 -->

## 해결하는 문제

<!-- TODO: 사용자 관점별 해결하는 문제 -->

## 사용자

| 역할 | 설명 | 주요 사용 기능 |
|------|------|--------------|
<!-- TODO: 사용자 페르소나 -->

## 핵심 가치

<!-- TODO: 제품이 전달하는 핵심 가치 3가지 -->

## 제품 원칙

<!-- TODO: 의사결정 시 우선순위 기준 -->

## 성공 지표

| 지표 | 설명 | 목표 |
|------|------|------|
<!-- TODO: 이 제품의 성공을 측정하는 기준 -->

## 경쟁 대안

| 대안 | 장점 | 한계 |
|------|------|------|
<!-- TODO: 기존 대안과 비교 -->

## 전략적 의도

<!-- TODO: 이 제품의 비즈니스 전략적 위치 -->

## Feature Roadmap

| 단계 | 기능 | 상태 |
|------|------|------|
| Phase 1 | <!-- TODO --> | 진행 중 |
| Phase 2 | <!-- TODO --> | 대기 |
```

---

## docs/DESIGN.md

```markdown
# {{PROJECT_TITLE}} - Design System

> 워크스페이스 디자인 컨텍스트([.impeccable.md](../../../.impeccable.md)) 기반.

## Colors

### Base
- Background: `bg-gray-50`
- Surface: `bg-white`
- Text Primary: `text-gray-900`
- Text Secondary: `text-gray-600`
- Text Muted: `text-gray-400`
- Border: `border-gray-200`

### Brand
- Primary: `bg-blue-600` / `text-blue-600`
- Primary Hover: `bg-blue-700`
- Primary Light: `bg-blue-50`
<!-- TODO: 프로젝트 브랜드 색상 -->

### Interactive
- Primary Button: `bg-blue-600 hover:bg-blue-700 text-white rounded-xl px-6 py-3 disabled:opacity-50`
- Secondary Button: `border border-gray-300 hover:bg-gray-50 rounded-xl px-6 py-3 disabled:opacity-50`
- Ghost Button: `hover:bg-gray-100 text-gray-600 rounded-xl px-4 py-2 disabled:opacity-50`
- Input: `rounded-xl border border-gray-200`

## Typography

- Font: Pretendard Variable (본문), Geist Mono (수치)
- Page Title (h2): `text-2xl font-bold text-gray-900`
- Section Title (h3): `text-lg font-semibold text-gray-800`
- Card Header (h4): `text-sm font-semibold text-gray-700`
- Body: `text-sm text-gray-600`
- Small Label: `text-xs text-gray-500 uppercase tracking-wide`
- **규칙**: 동일 레벨에서 font-bold/font-semibold 혼용 금지

## Spacing

- Page Padding: `p-6 md:p-8`
- Section Gap: `space-y-6`
- Card Padding: `p-6`
- Content Max Width: `max-w-3xl mx-auto`

## Components

### Card
\`\`\`
rounded-xl border border-gray-200 bg-white p-6 shadow-sm
hover: shadow-md transition-shadow (선택 가능 카드)
\`\`\`

<!-- TODO: 프로젝트 고유 컴포넌트 스타일 -->

## Animation

- 전환: `transition-all duration-300 ease-out`
- 토글: `transition-colors duration-150`
- 호버: `transition-shadow duration-200`
- **규칙**: 무한 반복 애니메이션 지양. `prefers-reduced-motion` 미디어 쿼리 대응 필수

## Responsive

- 기본: 태블릿/노트북 (1024px+)
- 브레이크포인트: `md:` (768px) 기준
- 터치 타겟: 최소 44px
```

---

## docs/FRONTEND.md

```markdown
# {{PROJECT_TITLE}} - Frontend Patterns

## 컴포넌트 구조

- 페이지 전용: `src/app/{feature}/_components/`
- 범용 UI: `src/components/ui/`
- 기능별 서브그룹: `_components/{subgroup}/` (규모 커지면)

## Server vs Client 컴포넌트

| 구분 | Server Component | Client Component |
|------|-----------------|------------------|
| 위치 | `page.tsx`, `layout.tsx` | `_components/*.tsx` (with "use client") |
| 데이터 | fetch, DB 접근 | props + useState |
| 용도 | 레이아웃, 라우팅, 데이터 페칭 | 인터랙션, 상태 관리 |

## 상태 관리

<!-- TODO: 상태 관리 패턴 (useState, useReducer, Context 등) -->

## 공유 훅

`src/lib/hooks/` 디렉토리에 커스텀 훅을 배치한다.

<!-- TODO: 프로젝트 공유 훅 문서화 -->
\`\`\`typescript
// 예시: useCountUp (숫자 애니메이션)
function useCountUp(target: number, options?: { duration?: number; active?: boolean }): number
\`\`\`

## 스타일링

- Tailwind CSS 유틸리티 클래스 사용
- 인라인 className만 사용
- 색상: docs/DESIGN.md 참조
- 디자인 토큰은 globals.css CSS 변수로 정의

## Forms & Validation

- 필수 필드 검증 후 다음 단계 허용
- 에러 표시: `text-xs text-gray-400` (필드 하단)
- 검증 함수는 `lib/validations.ts`에 공용 정의

## Accessibility

- 커스텀 컨트롤: `sr-only` input + `aria-hidden` 커스텀 UI
- 인터랙티브 요소: `role`, `tabIndex`, `aria-label` 필수
- 아코디언/토글: 네이티브 `<button>` 사용 (중첩 button 방지)
- 프로그레스: `role="progressbar"`, `aria-valuenow/min/max`
- 키보드: Enter/Space로 클릭 가능
- 색상 대비: WCAG AA (4.5:1) 준수
```

---

## docs/PLANS.md

```markdown
# {{PROJECT_TITLE}} - Plans

> 최종 업데이트: {{DATE}}

## 현재 우선순위

<!-- TODO: 지금 집중해야 할 항목 -->

## 로드맵

### Phase 1
<!-- TODO -->

### Phase 2
<!-- TODO -->

## 기술적 의사결정

<!-- 주요 기술 선택의 근거를 기록. "왜 X인가?" 형식. -->

### 왜 [기술/접근 방식]인가?
<!-- TODO: 선택 이유 + 대안 비교 -->

## 기술 부채

<!-- TODO: 알려진 기술 부채 목록 -->
```

---

## lib/types.ts

```typescript
// {{PROJECT_TITLE}} - 공유 타입 정의
```

---

## lib/constants.ts

```typescript
// {{PROJECT_TITLE}} - 공유 상수
```
