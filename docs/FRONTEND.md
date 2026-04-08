# Frontend Rules - claude-code-playbook

## HTML 규칙

- 시맨틱 태그 사용 (section, nav, article, aside)
- 접근성: aria-label, role 속성 포함
- 모든 인터랙티브 요소에 키보드 접근 가능

## CSS 규칙

- Tailwind CSS 유틸리티 클래스 우선
- 커스텀 CSS는 `<style>` 태그 내 최소화
- 다크 모드: `dark:` 프리픽스 사용
- 반응형: mobile-first (`md:`, `lg:` 브레이크포인트)

## JavaScript 규칙

- Vanilla JS만 사용 (프레임워크 없음)
- 이벤트 위임 패턴 활용
- localStorage로 상태 관리 (체크리스트, 다크모드)
- 코드 복사: Clipboard API 사용
