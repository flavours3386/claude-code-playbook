# Backend Templates

Python / Node.js 백엔드 프로젝트에 추가되는 템플릿. 공통 템플릿(common-templates.md)의 문서 구조를 기반으로 소스 구조와 백엔드 전용 문서를 추가.

---

## CLAUDE.md 백엔드 변형

common-templates.md의 CLAUDE.md에서 다음을 교체/추가.

### Python 백엔드

주요 명령어:
```markdown
\`\`\`bash
python -m src.main           # 서버 실행
pytest                       # 테스트
pytest --cov=src             # 커버리지
\`\`\`
```

폴더 구조:
```markdown
\`\`\`
src/
├── api/routes/    # 라우트 핸들러
├── models/        # Pydantic / ORM 모델
├── services/      # 비즈니스 로직
├── utils/         # 유틸리티
├── config.py      # 설정
└── main.py        # 엔트리포인트
tests/             # 테스트
\`\`\`
```

코드 스타일 교체:
```markdown
## 코드 스타일

- Python {{PYTHON_VERSION}}, type hints 필수
- 함수, 변수: snake_case / 클래스: PascalCase / 상수: UPPER_SNAKE_CASE
- Pydantic 모델로 요청/응답 스키마 정의
- 주석: 한글, 복잡한 비즈니스 로직에만
```

### Node.js 백엔드

주요 명령어:
```markdown
\`\`\`bash
npm run dev          # 개발 서버 (nodemon/tsx)
npm run build        # 빌드
npm run start        # 프로덕션 실행
npm run test         # 테스트
\`\`\`
```

폴더 구조:
```markdown
\`\`\`
src/
├── routes/        # 라우트 핸들러
├── models/        # 데이터 모델
├── services/      # 비즈니스 로직
├── middleware/     # 미들웨어
├── utils/         # 유틸리티
├── config.ts      # 설정
└── index.ts       # 엔트리포인트
tests/             # 테스트
\`\`\`
```

---

## AGENTS.md 백엔드 변형

common-templates.md의 AGENTS.md에서 다음을 교체.

Golden Principles:
```markdown
## Golden Principles

1. **레이어 분리**: routes → services → models. 역방향 import 금지
2. **라우트는 얇게**: 요청 파싱 + 응답 반환만. 비즈니스 로직은 services에
3. **설정 중앙화**: `config.py` / `config.ts`에서 환경변수 로드. 하드코딩 금지
4. **입력 검증 필수**: 모든 API 엔드포인트에서 요청 데이터 검증
5. **에러 응답 통일**: `{ error: { code: string, message: string } }` 포맷
6. **UI 텍스트 한국어**, 코드(변수/함수명) 영어
```

Docs Map에서 DESIGN.md, FRONTEND.md 제거, BACKEND.md + API.md 추가:
```markdown
| [docs/BACKEND.md](docs/BACKEND.md) | 코드 패턴, 레이어 규칙, 컨벤션 | 기능 추가 시 |
| [docs/API.md](docs/API.md) | API 엔드포인트 명세 | 기능 추가 시 |
| [docs/SECURITY.md](docs/SECURITY.md) | 인증, 데이터 보호 | 분기 |
| [docs/RELIABILITY.md](docs/RELIABILITY.md) | 에러 처리, 테스트 전략 | 분기 |
```

---

## ARCHITECTURE.md 백엔드 변형

### Python

```markdown
# {{PROJECT_TITLE}} - Architecture

## 의존성 레이어

\`\`\`
Layer 0: Config         (src/config.py)
Layer 1: Models         (src/models/)
Layer 2: Services       (src/services/)
Layer 3: Routes         (src/api/routes/)
Layer 4: App            (src/main.py)
\`\`\`

**규칙**: 상위 레이어는 하위 레이어만 import. 역방향 금지.

## 데이터 흐름

\`\`\`
Client Request
  → Route Handler (src/api/routes/)
    → Input Validation (Pydantic model)
    → Service (src/services/)
      → Model / DB Query (src/models/)
    → Response
  ← JSON Response
\`\`\`

## API 응답 포맷

\`\`\`json
// 성공
{ "data": T }

// 에러
{ "error": { "code": "string", "message": "string" } }

// 목록
{ "data": [T], "total": number }
\`\`\`
```

### Node.js

```markdown
# {{PROJECT_TITLE}} - Architecture

## 의존성 레이어

\`\`\`
Layer 0: Config         (src/config.ts)
Layer 1: Models         (src/models/)
Layer 2: Services       (src/services/)
Layer 3: Middleware      (src/middleware/)
Layer 4: Routes         (src/routes/)
Layer 5: App            (src/index.ts)
\`\`\`

**규칙**: 상위 레이어는 하위 레이어만 import. 역방향 금지.

## 데이터 흐름

\`\`\`
Client Request
  → Middleware (auth, logging, cors)
    → Route Handler (src/routes/)
      → Input Validation
      → Service (src/services/)
        → Model / DB Query (src/models/)
      → Response
    ← JSON Response
\`\`\`
```

---

## docs/BACKEND.md

```markdown
# {{PROJECT_TITLE}} - Backend Patterns

## 레이어 구조

\`\`\`
Routes (요청/응답 처리) → Services (비즈니스 로직) → Models (데이터)
\`\`\`

- **Routes**: 요청 파싱, 입력 검증, 응답 반환만. 비즈니스 로직 금지.
- **Services**: 핵심 비즈니스 로직. DB 접근은 models 통해서만.
- **Models**: 데이터 스키마, ORM 모델, 타입 정의.

## 서비스 작성 규칙

- 하나의 서비스 파일 = 하나의 도메인 (user_service, order_service)
- 서비스 함수는 순수 로직 중심. HTTP 요청/응답 객체 접근 금지
- 다른 서비스 호출 가능하나 순환 의존 금지

## 에러 처리 컨벤션

- 비즈니스 에러: 커스텀 예외 클래스 사용 (NotFoundError, ValidationError 등)
- 시스템 에러: 전역 에러 핸들러에서 캐치
- 에러 응답 포맷 통일: `{ error: { code, message } }`

## 입력 검증

- Python: Pydantic 모델로 자동 검증 (라우트 파라미터에 타입 선언)
- Node.js: zod 스키마로 검증 (미들웨어 또는 라우트 진입점)
- 검증 실패 시 400 + 구체적 에러 메시지

## DB 접근 패턴

<!-- TODO: ORM 선택 후 패턴 정의 (SQLAlchemy, Prisma, TypeORM 등) -->

## 설정 관리

- 모든 환경변수는 `config` 모듈에서 로드
- 코드에서 직접 `os.getenv()` / `process.env` 사용 금지
- `.env.example`에 필요한 변수 목록 유지
```

---

## docs/API.md

```markdown
# {{PROJECT_TITLE}} - API Reference

## Base URL

\`\`\`
개발: http://localhost:8000
프로덕션: <!-- TODO -->
\`\`\`

## 인증

<!-- TODO: 인증 방식 (Bearer Token, API Key 등) -->

## 공통 응답 포맷

\`\`\`json
// 성공
{ "data": { ... } }

// 에러
{ "error": { "code": "VALIDATION_ERROR", "message": "..." } }
\`\`\`

## 엔드포인트

### Resource 1

<!-- TODO: 엔드포인트 명세 -->

| Method | Path | 설명 |
|--------|------|------|
| GET | /api/v1/{resource} | 목록 조회 |
| POST | /api/v1/{resource} | 생성 |
| GET | /api/v1/{resource}/:id | 상세 조회 |
| PUT | /api/v1/{resource}/:id | 수정 |
| DELETE | /api/v1/{resource}/:id | 삭제 |
```

---

## docs/SECURITY.md (백엔드용)

```markdown
# {{PROJECT_TITLE}} - Security

## 인증

<!-- TODO: 인증 방식 (JWT, API Key, OAuth 등) -->

## 입력 검증

- 모든 API 라우트에서 요청 데이터 검증 필수
- Python: Pydantic 모델로 자동 검증
- Node.js: zod / joi 등 스키마 검증

## CORS

<!-- TODO: 허용 origin 목록 -->

## 민감 데이터

- 환경변수: `.env` 파일로 관리 (`.env.example` 제공)
- 시크릿: `secrets-vault/{{PROJECT_NAME}}/`에서 중앙 관리
- 코드에 하드코딩 금지
- 로그에 민감 정보 출력 금지
```

---

## docs/RELIABILITY.md (백엔드용)

```markdown
# {{PROJECT_TITLE}} - Reliability

## 에러 처리

- 전역 에러 핸들러로 예외 캐치
- 비즈니스 에러와 시스템 에러 구분
- 클라이언트에 스택 트레이스 노출 금지 (프로덕션)

## 로깅

<!-- TODO: 로깅 전략 (레벨, 포맷, 출력 대상) -->

## 테스트

- 단위 테스트: services 레이어 중심
- 통합 테스트: API 엔드포인트 요청/응답
- 테스트 DB: 본 DB와 분리

## 헬스체크

\`\`\`
GET /health → { "status": "ok" }
\`\`\`
```

---

## Python 소스 파일

### src/config.py

```python
"""{{PROJECT_TITLE}} - 설정"""

import os
from dataclasses import dataclass


@dataclass(frozen=True)
class Config:
    """환경변수 기반 설정"""
    HOST: str = os.getenv("HOST", "0.0.0.0")
    PORT: int = int(os.getenv("PORT", "8000"))
    DEBUG: bool = os.getenv("DEBUG", "false").lower() == "true"
    # TODO: 추가 설정


config = Config()
```

### src/main.py (FastAPI)

```python
"""{{PROJECT_TITLE}} - 엔트리포인트"""

from fastapi import FastAPI
from src.config import config

app = FastAPI(title="{{PROJECT_TITLE}}")


@app.get("/health")
async def health():
    return {"status": "ok"}


# TODO: 라우터 등록
# from src.api.routes import router
# app.include_router(router, prefix="/api/v1")

if __name__ == "__main__":
    import uvicorn
    uvicorn.run("src.main:app", host=config.HOST, port=config.PORT, reload=config.DEBUG)
```

### tests/conftest.py

```python
"""{{PROJECT_TITLE}} - 테스트 설정"""

import pytest
from fastapi.testclient import TestClient
from src.main import app


@pytest.fixture
def client():
    return TestClient(app)
```

### requirements.txt

```
fastapi>=0.115.0
uvicorn>=0.34.0
pydantic>=2.10.0
python-dotenv>=1.0.0
pytest>=8.0.0
httpx>=0.28.0
```

### .env.example

```
HOST=0.0.0.0
PORT=8000
DEBUG=true
```

---

## Node.js 소스 파일

### src/config.ts

```typescript
// {{PROJECT_TITLE}} - 설정

import 'dotenv/config';

export const config = {
  HOST: process.env.HOST ?? '0.0.0.0',
  PORT: Number(process.env.PORT ?? 3000),
  NODE_ENV: process.env.NODE_ENV ?? 'development',
  // TODO: 추가 설정
} as const;
```

### src/index.ts (Express)

```typescript
// {{PROJECT_TITLE}} - 엔트리포인트

import express from 'express';
import cors from 'cors';
import { config } from './config';

const app = express();

app.use(cors());
app.use(express.json());

app.get('/health', (_req, res) => {
  res.json({ status: 'ok' });
});

// TODO: 라우터 등록
// import { router } from './routes';
// app.use('/api/v1', router);

app.listen(config.PORT, config.HOST, () => {
  console.log(`Server running on ${config.HOST}:${config.PORT}`);
});

export { app };
```

### package.json (템플릿)

```json
{
  "name": "{{PROJECT_NAME}}",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "tsx watch src/index.ts",
    "build": "tsc",
    "start": "node dist/index.js",
    "test": "jest"
  },
  "dependencies": {
    "cors": "^2.8.5",
    "dotenv": "^16.4.0",
    "express": "^5.1.0"
  },
  "devDependencies": {
    "@types/cors": "^2.8.17",
    "@types/express": "^5.0.0",
    "@types/node": "^20.0.0",
    "tsx": "^4.21.0",
    "typescript": "^5.3.0"
  }
}
```

### .env.example

```
HOST=0.0.0.0
PORT=3000
NODE_ENV=development
```
