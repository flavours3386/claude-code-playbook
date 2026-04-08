# Architecture - claude-code-playbook

## 개요

단일 HTML 파일(index.html)로 구성된 정적 플레이북 사이트.
Tailwind CSS CDN과 Vanilla JS로 인터랙션 처리.

## 페이지 구조

```
index.html
├── <head> ─── Tailwind CDN, 메타 태그, 인라인 스타일
├── <nav> ──── 좌측 사이드바 네비게이션 (목차)
├── <main> ─── 콘텐츠 섹션들
│   ├── #overview ──────── 개요
│   ├── #prerequisites ─── 사전 준비 (Mac/Windows 탭)
│   ├── #installation ──── Claude Code 설치
│   ├── #global-config ─── 글로벌 설정
│   ├── #omc-setup ─────── OMC 설치
│   ├── #plugins ──────── 플러그인 설치
│   ├── #mcp-servers ───── MCP 서버 설정
│   ├── #hooks ─────────── 훅 시스템
│   ├── #skills ────────── 스킬 가이드
│   ├── #workflows ─────── 일상 워크플로우
│   ├── #troubleshooting ─ 트러블슈팅
│   └── #quick-start ───── 빠른 시작 가이드 (3단계 + 올인원 프롬프트)
├── <footer>
└── <script> ── 인터랙션 로직 (탭 전환, 복사, 다크모드, 체크리스트)
```

## 인터랙션 패턴

- **OS 탭 전환**: Mac/Windows 명령어 분기
- **코드 복사**: 클릭 시 클립보드 복사
- **다크/라이트 모드**: 시스템 설정 감지 + 수동 토글
- **체크리스트**: localStorage 기반 진행률 추적
- **사이드바 하이라이트**: IntersectionObserver로 현재 섹션 표시

## 배포

```
Vercel Static Deployment → https://claude-code-playbook-rosy.vercel.app
GitHub: flavours3386/claude-code-playbook (자동 배포 연동)
├── vercel.json (라우팅, 헤더)
└── index.html (엔트리포인트)
```

## 규모

- ~2,545줄 / ~170KB (단일 파일)
- 12개 섹션, 24개 체크리스트 항목
- 인터랙티브 컴포넌트 6종 (사이드바, OS탭, 코드복사, 체크리스트, 다크모드, 접이식패널)
- 반응형 CSS: 2개 브레이크포인트 (767px, 399px) 미디어쿼리

## 의존성

- Tailwind CSS v4 (CDN)
- Pretendard Variable 폰트 (CDN)
- JetBrains Mono 폰트 (Google Fonts CDN)
- 외부 빌드 도구 없음
