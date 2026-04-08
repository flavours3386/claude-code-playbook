# Frontend Rules - claude-code-playbook

## HTML 규칙

- 시맨틱 태그 사용 (section, nav, article, aside)
- 접근성: aria-label, role 속성 포함
- 모든 인터랙티브 요소에 키보드 접근 가능

## CSS 규칙

- Tailwind CSS 유틸리티 클래스 우선
- 커스텀 CSS는 `<style>` 태그 내 최소화
- 다크 모드: `dark:` 프리픽스 사용
- 반응형: `@media (max-width: 767px)` / `@media (max-width: 399px)` 미디어쿼리
- 인라인 스타일 오버라이드 시 `!important` 사용 (미디어쿼리 내에서만)
- 테이블은 부모 `overflow-x: auto`로 가로 스크롤 처리
- 그리드는 모바일에서 `grid-template-columns: 1fr` 강제

## JavaScript 규칙

- Vanilla JS만 사용 (프레임워크 없음)
- 이벤트 위임 패턴 활용
- localStorage로 상태 관리 (체크리스트, 다크모드)
- 코드 복사: Clipboard API 사용
