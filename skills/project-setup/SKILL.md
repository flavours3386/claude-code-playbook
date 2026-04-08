---
name: project-setup
description: "새 프로젝트의 표준 문서 구조(CLAUDE.md, AGENTS.md, ARCHITECTURE.md, docs/)와 폴더 구조를 스캐폴딩. /project-setup 또는 '새 프로젝트 세팅', '프로젝트 구조 만들어줘' 시 트리거. 4가지 모드: 클라이언트 전용 프론트엔드, 풀스택(Next.js+DB), Python 백엔드, Node.js 백엔드."
---

# Project Setup

새 프로젝트의 표준 구조를 스캐폴딩한다.

## Workflow

### Step 1: 정보 수집

AskUserQuestion으로 다음을 확인:

1. **프로젝트명** (영문 kebab-case)
2. **한줄 설명** (한국어)
3. **프로젝트 타입**:
   - 클라이언트 전용: static export, DB/API 없음
   - 풀스택: Next.js + DB (Prisma/Drizzle), API 라우트 포함
   - Python 백엔드: FastAPI/Flask, REST API 서버
   - Node.js 백엔드: Express/standalone, REST API 서버
4. **기술 스택** (타입별 기본값 제안)

### Step 2: 디렉토리 생성

타입에 따라 구조 생성. 문서 구조는 모든 타입에 공통.

#### 공통 문서 구조 (전 타입)

```
{project-name}/
├── CLAUDE.md
├── AGENTS.md
├── ARCHITECTURE.md
└── docs/
    ├── PRODUCT_SENSE.md
    ├── PLANS.md
    ├── SECURITY.md          # 백엔드/풀스택만
    ├── RELIABILITY.md       # 백엔드/풀스택만
    ├── design-docs/         # .gitkeep
    └── exec-plans/          # .gitkeep
```

프론트엔드(클라이언트/풀스택)이면 추가:
```
    ├── DESIGN.md
    └── FRONTEND.md
```

백엔드(Python/Node.js)이면 추가:
```
    └── API.md               # API 엔드포인트 명세
```

#### 클라이언트 전용 소스 구조

```
src/
├── app/
│   ├── layout.tsx, page.tsx, globals.css
├── components/
│   └── ui/
└── lib/
    ├── types.ts
    ├── constants.ts
    └── hooks/               # 커스텀 훅
```

#### 풀스택 소스 구조

클라이언트 전용 + 아래 추가:
```
src/
├── app/
│   ├── loading.tsx, error.tsx
│   └── api/
└── lib/
    ├── db/
    │   ├── index.ts         # DB 클라이언트 (Prisma/Drizzle)
    │   └── schema.ts        # 테이블 스키마
    ├── api-response.ts
    └── validations.ts       # Zod 검증 스키마
```

#### Python 백엔드 소스 구조

```
src/
├── api/
│   └── routes/              # 라우트 핸들러
├── models/                  # Pydantic 모델 / ORM 모델
├── services/                # 비즈니스 로직
├── utils/                   # 유틸리티
├── config.py                # 설정 (환경변수 로드)
└── main.py                  # 엔트리포인트
tests/
├── conftest.py
└── test_api/
requirements.txt
.env.example
```

#### Node.js 백엔드 소스 구조

```
src/
├── routes/                  # 라우트 핸들러
├── models/                  # 데이터 모델
├── services/                # 비즈니스 로직
├── middleware/               # 미들웨어
├── utils/                   # 유틸리티
├── config.ts                # 설정 (환경변수 로드)
└── index.ts                 # 엔트리포인트
tests/
package.json
tsconfig.json
.env.example
```

### Step 3: 템플릿 적용

각 파일의 내용은 레퍼런스 참조:
- **공통 템플릿**: [references/common-templates.md](references/common-templates.md)
- **풀스택 추가**: [references/fullstack-templates.md](references/fullstack-templates.md)
- **백엔드 추가**: [references/backend-templates.md](references/backend-templates.md)

템플릿 내 플레이스홀더를 치환:
- `{{PROJECT_NAME}}` → 프로젝트명 (kebab-case)
- `{{PROJECT_TITLE}}` → 프로젝트 제목 (한국어)
- `{{DESCRIPTION}}` → 한줄 설명
- `{{DATE}}` → 오늘 날짜 (YYYY-MM-DD)
- `{{TECH_STACK}}` → 선택한 기술 스택
- `{{PYTHON_VERSION}}` → Python 버전 (백엔드, 기본 3.11)

### Step 4: 워크스페이스 연동

현재 디렉토리가 PJT 워크스페이스 내부이면:
1. `PJT/CLAUDE.md`의 독립 프로젝트 테이블에 새 프로젝트 추가
2. `.impeccable.md`가 워크스페이스 루트에 있으면 AGENTS.md Docs Map에 참조 추가 (프론트엔드만)

### Step 5: 확인

생성된 파일 목록을 출력하고 다음 단계 안내:
- `cd {project-name}` 후 개발 시작
- 패키지 초기화 안내 (타입별)
- CLAUDE.md의 TODO 항목 채우기

## Golden Principles

### 프론트엔드 (클라이언트/풀스택)

1. **컴포넌트 co-location**: 페이지 전용은 `_components/`에
2. **범용 UI만 `components/ui/`에**: 범용 아니면 `_components/`
3. **공유 상수 한 곳에**: `lib/constants.ts`
4. **비즈니스 로직 분리**: `lib/{domain}.ts`. 컴포넌트에 계산 금지
5. **(풀스택) API 응답 헬퍼**: `api-response.ts` 사용 필수
6. **(풀스택) 검증은 Zod**: `validations.ts`에서 Zod 스키마로 클라이언트+서버 공유
7. **(풀스택) DB import**: `@/lib/db`에서만 import (ORM 무관)
8. **의존성 방향**: Types -> Constants -> Domain Logic -> UI -> Pages

### 백엔드 (Python/Node.js)

1. **레이어 분리**: routes(요청 처리) -> services(비즈니스 로직) -> models(데이터)
2. **라우트는 얇게**: 라우트 핸들러에 비즈니스 로직 금지. services에 위임
3. **설정 중앙화**: `config.py` / `config.ts`에서 환경변수 로드. 코드에 하드코딩 금지
4. **입력 검증 필수**: 모든 API 엔드포인트에서 요청 데이터 검증
5. **에러 응답 통일**: 표준 에러 포맷 사용 `{ error: { code, message } }`
6. **의존성 방향**: Config -> Models -> Services -> Routes -> App

## 문서 역할 구분

| 문서 | 역할 | 적용 타입 |
|------|------|-----------|
| **CLAUDE.md** | 진입 계약서 (상세 컨텍스트, 규칙, 상태) | 전체 |
| **AGENTS.md** | 문서 목차 + 핵심 원칙 (3초 안에 전체 파악) | 전체 |
| **ARCHITECTURE.md** | 의존성 레이어, 데이터 흐름, 모듈 경계 | 전체 |
| **docs/PRODUCT_SENSE.md** | 제품 방향, 사용자, 핵심 가치 | 전체 |
| **docs/PLANS.md** | 우선순위, 로드맵, 기술 부채, 기술적 의사결정 | 전체 |
| **docs/DESIGN.md** | 디자인 시스템 (색상, 타이포, 컴포넌트) | 프론트엔드 |
| **docs/FRONTEND.md** | 컴포넌트 패턴, Server/Client 규칙 | 프론트엔드 |
| **docs/BACKEND.md** | 코드 패턴, 레이어 규칙, 컨벤션 | 백엔드 |
| **docs/API.md** | API 엔드포인트 명세, 요청/응답 포맷 | 백엔드 |
| **docs/SECURITY.md** | 인증, 입력 검증, 민감 데이터 | 백엔드/풀스택 |
| **docs/RELIABILITY.md** | 에러 처리, 검증 전략, 테스트 | 백엔드/풀스택 |
| **docs/design-docs/** | 설계 의사결정 기록 (DD-001~) | 전체 |
| **docs/exec-plans/** | 실행 계획, 테스트 케이스 (EP-001~) | 전체 |
