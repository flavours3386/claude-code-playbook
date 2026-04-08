# claude-code-playbook

팀원용 Claude Code 셋업 플레이북 - HTML 정적 사이트

## 프로젝트 개요

- **목적**: 팀원이 Mac/Windows에서 동일한 Claude Code 환경을 구축할 수 있는 인터랙티브 가이드
- **타입**: 클라이언트 전용 (정적 HTML)
- **배포**: Vercel 정적 배포 (https://claude-code-playbook-rosy.vercel.app)
- **GitHub**: https://github.com/flavours3386/claude-code-playbook
- **생성일**: 2026-04-08

## 기술 스택

- HTML5 (단일 페이지)
- Tailwind CSS (CDN)
- Vanilla JavaScript (인터랙션)
- Vercel (정적 배포)

## 주요 명령어

```bash
# 로컬 미리보기
npx serve .

# Vercel 배포
vercel

# 프로덕션 배포
vercel --prod
```

## 폴더 구조

```
claude-code-playbook/
├── CLAUDE.md              # 프로젝트 진입 계약서
├── AGENTS.md              # 문서 목차 + 핵심 원칙
├── ARCHITECTURE.md        # 아키텍처 문서
├── .impeccable.md         # 디자인 컨텍스트 (impeccable)
├── index.html             # 메인 플레이북 (2,029줄, 130KB)
├── vercel.json            # Vercel 배포 설정
├── .gitignore
└── docs/
    ├── PRODUCT_SENSE.md   # 제품 방향
    ├── PLANS.md           # 우선순위, 로드맵
    ├── DESIGN.md          # 디자인 시스템
    ├── FRONTEND.md        # 프론트엔드 규칙
    ├── design-docs/       # 설계 의사결정 기록
    └── exec-plans/        # 실행 계획
```

## 코드 컨벤션

- 한국어 콘텐츠, 영문 코드
- Tailwind CSS 유틸리티 클래스 사용
- 시맨틱 HTML 태그 활용
- 민감 정보(토큰, API 키)는 플레이스홀더로 대체

## Design Context

상세: [.impeccable.md](.impeccable.md)

### Users
- 세일즈팀 팀원 (개발 경험 다양, 비개발자 포함)
- Mac/Windows 사용자, Claude Code 첫 사용자
- Job: 가이드만 따라가면 30분 내 동일한 환경 완성

### Brand Personality
따뜻한(Warm), 신뢰감 있는(Trustworthy), 명료한(Clear)

### Aesthetic Direction
- Anthropic/Claude 공식 브랜드 톤 (#D97757 + Stone neutrals)
- 라이트 모드 기본, 다크 모드 지원
- 코드 블록은 항상 다크 테마

### Design Principles
1. 따라하기가 곧 디자인이다 (시각적 계층 = 실행 순서)
2. 코드는 주인공 (코드 블록이 가장 눈에 띄는 요소)
3. OS 분기는 한 눈에 (Mac/Windows 탭 분리)
4. 따뜻한 기술 문서 (Anthropic 톤)
5. 진행 감각 (체크리스트 + 네비게이션)

## 최근 변경사항

- 2026-04-08: 프로젝트 초기 셋업, 디자인 시스템 구성
- 2026-04-08: index.html 플레이북 본문 작성 (12섹션, 24 체크리스트, 6 인터랙티브 컴포넌트)
- 2026-04-08: Critic 리뷰 P1 수정 (접근성, 체크리스트 추가, aria-hidden)
- 2026-04-08: CLAUDE.md 전문 반영, 커스텀 스킬 3개 추가
- 2026-04-08: Content Auditor 검증 수정 (매직 키워드 4개, CLAUDE.md 6곳 복원)
- 2026-04-08: 프롬프트 기반 셋업 구조 도입 (섹션 4-7, 12)
- 2026-04-08: GitHub 레포 생성 + Vercel 프로덕션 배포
