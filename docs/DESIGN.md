# Design System - claude-code-playbook

상세 디자인 컨텍스트: [.impeccable.md](../.impeccable.md)

## 색상 체계

### Primary (Anthropic Orange)
| 토큰 | 라이트 | 다크 | 용도 |
|------|--------|------|------|
| primary | #D97757 | #D97757 | 버튼, 링크, 활성 탭, 진행률 바 |
| primary-hover | #C4684A | #B85A3D | 호버 상태 |
| primary-light | #FEF3EE | #3C2415 | 배경 강조 |

### Neutrals (Stone)
| 토큰 | 라이트 | 다크 | 용도 |
|------|--------|------|------|
| bg | #FAFAF9 | #1C1917 | 페이지 배경 |
| surface | #FFFFFF | #292524 | 카드, 사이드바 |
| surface-elevated | #FFFFFF | #44403C | 호버 카드 |
| border | #E7E5E4 | #44403C | 구분선, 카드 보더 |
| text-primary | #1C1917 | #FAFAF9 | 제목, 본문 |
| text-secondary | #78716C | #A8A29E | 부가 설명 |
| text-tertiary | #787270 | #6B6966 | 비활성 텍스트 (WCAG AA 대비 보정) |

### Semantic
| 토큰 | 색상 | 배경 | 용도 |
|------|------|------|------|
| success | #16A34A | #F0FDF4 | Tip 콜아웃, 완료 |
| warning | #CA8A04 | #FEFCE8 | Warning 콜아웃 |
| danger | #DC2626 | #FEF2F2 | Danger 콜아웃 |
| info | #2563EB | #EFF6FF | Info 콜아웃 |

## 타이포그래피

- **본문**: Pretendard Variable > system-ui > sans-serif
- **코드**: JetBrains Mono > ui-monospace > SF Mono > Cascadia Code
- **스케일**: xs(12) / sm(14) / base(16) / lg(18) / xl(20) / 2xl(24) / 3xl(30)
- **제목**: font-weight 700, tracking-tight

## 레이아웃

- **사이드바**: 280px (lg+), 모바일 오버레이
- **콘텐츠**: max-w-3xl (768px)
- **전체**: max-w-7xl (1280px)
- **카드 radius**: rounded-xl (12px)
- **코드 블록 radius**: rounded-lg (8px)

## 컴포넌트 목록

| 컴포넌트 | 설명 |
|----------|------|
| Code Block | 항상 다크 배경, 복사 버튼, 언어 라벨 |
| OS Tab | Mac/Windows 전환, border-bottom 스타일 |
| Checklist | 체크박스 + 진행률 바, localStorage |
| Callout | Tip/Warning/Danger/Info, 좌측 컬러 보더 |
| Sidebar Nav | 섹션 그룹, IntersectionObserver 하이라이트 |
| Dark Mode Toggle | 시스템 감지 + 수동 토글 |

## 디자인 원칙

1. **따라하기가 곧 디자인이다** - 시각적 계층 = 실행 순서
2. **코드는 주인공** - 코드 블록이 가장 눈에 띄는 요소
3. **OS 분기는 한 눈에** - 탭으로 깔끔하게 분리
4. **따뜻한 기술 문서** - Anthropic 톤, 기술적이되 인간적
5. **진행 감각** - 체크리스트 + 네비게이션으로 현재 위치 인지
