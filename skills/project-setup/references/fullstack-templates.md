# Fullstack Templates

풀스택 프로젝트에 추가되는 템플릿. 공통 템플릿(common-templates.md)에 병합하여 사용.

---

## CLAUDE.md 추가 섹션

CLAUDE.md의 주요 명령어에 추가:

```markdown
npx drizzle-kit push     # DB 스키마 푸시
npx drizzle-kit generate # 마이그레이션 생성
```

> ORM에 따라 명령어 변경. Prisma: `npx prisma db push`, `npx prisma migrate dev`

폴더 구조에 추가:
```markdown
src/
├── app/
│   └── api/         # REST API 라우트
└── lib/
    ├── db/
    │   ├── index.ts      # DB 클라이언트 (Prisma/Drizzle)
    │   └── schema.ts     # 테이블 스키마
    ├── api-response.ts   # API 응답 헬퍼
    └── validations.ts    # Zod 검증 스키마 (클라이언트+서버 공용)
```

---

## AGENTS.md 추가 섹션

Golden Principles에 추가:

```markdown
6. **API 응답 헬퍼 사용 필수**: `lib/api-response.ts`의 `successResponse`, `errorResponse`, `listResponse` 사용. 직접 `Response.json()` 금지
7. **검증은 Zod**: `lib/validations.ts`에서 Zod 스키마로 클라이언트+서버 검증
8. **DB import**: 반드시 `@/lib/db` 경로 사용 (ORM 무관)
9. **데이터 페칭**: Server Component에서만. Client Component에서 fetch 금지
```

Docs Map에 추가:

```markdown
| [docs/SECURITY.md](docs/SECURITY.md) | 인증, 데이터 보호 | 분기 |
| [docs/RELIABILITY.md](docs/RELIABILITY.md) | 에러 처리, 테스트 전략 | 분기 |
```

---

## ARCHITECTURE.md 추가 섹션

의존성 레이어에 추가:

```markdown
Layer 2.5: Validations  (lib/validations.ts) — Zod 스키마
Layer 2.7: API Response (lib/api-response.ts)
Layer 2.8: DB Client    (lib/db/)
```

데이터 흐름:

```markdown
## 데이터 흐름

\`\`\`
Client Component
  → fetch('/api/{resource}')
    → API Route (app/api/{resource}/route.ts)
      → Zod Validation (lib/validations.ts)
      → DB Query (lib/db/)
      → Response Helper (lib/api-response.ts)
    ← { data: T } | { error: { code, message } }
\`\`\`

## API 응답 포맷

\`\`\`typescript
// 성공
{ data: T }

// 에러
{ error: { code: string, message: string } }

// 목록
{ data: T[], total: number }
\`\`\`
```

Server Architecture:

```markdown
## Server Architecture

### API Routes

| Method | Path | 설명 |
|--------|------|------|
<!-- TODO: API 엔드포인트 추가 -->

### DB Schema

\`\`\`
<!-- TODO: 테이블 구조 추가 -->
{table_name}
├── id (UUID, PK)
├── ...
└── created_at (TIMESTAMP)
\`\`\`

### 외부 연동

<!-- TODO: 외부 API/서비스 연동이 있으면 흐름 기술 -->

## 확장 포인트

<!-- TODO: 향후 확장 가능한 지점 기록 -->
```

---

## docs/SECURITY.md

```markdown
# {{PROJECT_TITLE}} - Security

## 인증

<!-- TODO: 인증 방식 (NextAuth, 커스텀 등) -->

## 입력 검증

- 모든 API 라우트에서 `lib/validations.ts`의 Zod 스키마로 검증 필수
- 클라이언트 + 서버 이중 검증

## 민감 데이터

- 환경변수: `.env` 파일로 관리
- 시크릿: `secrets-vault/{project-name}/`에서 중앙 관리
- 코드에 하드코딩 금지
- 환경변수 설정 시 `printf` 사용 권장 (trailing newline 방지)
```

---

## docs/RELIABILITY.md

```markdown
# {{PROJECT_TITLE}} - Reliability

## 에러 처리

- API: try/catch + `errorResponse()` 헬퍼
- 컴포넌트: `error.tsx` 경계
- 로딩: `loading.tsx` 폴백

## 검증 전략

- 클라이언트: 폼 제출 전 Zod 스키마 검증
- 서버: API 라우트에서 동일 Zod 스키마로 재검증

## 테스트

<!-- TODO: 테스트 전략 -->
```

---

## lib/db/index.ts (ORM 선택에 따라)

### Drizzle ORM (Neon)

```typescript
import { neon } from '@neondatabase/serverless';
import { drizzle } from 'drizzle-orm/neon-http';
import * as schema from './schema';

// Lazy 초기화 (빌드 시 DATABASE_URL 없을 수 있음)
const getDb = () => {
  const sql = neon(process.env.DATABASE_URL!);
  return drizzle(sql, { schema });
};

// Proxy 패턴: 접근 시점에 초기화
export const db = new Proxy({} as ReturnType<typeof getDb>, {
  get(_, prop) {
    const instance = getDb();
    return Reflect.get(instance, prop);
  },
});
```

### Prisma

```typescript
import { PrismaClient } from '@prisma/client';

const globalForPrisma = globalThis as unknown as {
  prisma: PrismaClient | undefined;
};

export const db = globalForPrisma.prisma ?? new PrismaClient();

if (process.env.NODE_ENV !== 'production') globalForPrisma.prisma = db;
```

---

## lib/db/schema.ts

### Drizzle ORM

```typescript
import { pgTable, uuid, text, timestamp, jsonb, integer, boolean } from 'drizzle-orm/pg-core';

// TODO: 테이블 정의
// export const examples = pgTable('examples', {
//   id: uuid('id').defaultRandom().primaryKey(),
//   name: text('name').notNull(),
//   data: jsonb('data'),
//   createdAt: timestamp('created_at').defaultNow().notNull(),
// });
```

### Prisma

```
// schema.prisma 파일을 사용. 이 파일은 비워둔다.
```

---

## lib/api-response.ts

```typescript
export function successResponse<T>(data: T, status = 200) {
  return Response.json({ data }, { status });
}

export function errorResponse(code: string, message: string, status = 400) {
  return Response.json({ error: { code, message } }, { status });
}

export function listResponse<T>(data: T[], total: number) {
  return Response.json({ data, total });
}
```

---

## lib/validations.ts

```typescript
import { z } from 'zod';

// TODO: 도메인별 Zod 스키마 추가
// export const createExampleSchema = z.object({
//   name: z.string().min(1),
//   data: z.record(z.unknown()).optional(),
// });
```

---

## app/loading.tsx

```tsx
export default function Loading() {
  return (
    <div className="flex h-full items-center justify-center">
      <div className="h-8 w-8 animate-spin rounded-full border-4 border-muted border-t-primary" />
    </div>
  );
}
```

---

## app/error.tsx

```tsx
'use client';

export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string };
  reset: () => void;
}) {
  return (
    <div className="flex h-full flex-col items-center justify-center gap-4">
      <h2 className="text-lg font-semibold">오류가 발생했습니다</h2>
      <p className="text-sm text-muted-foreground">{error.message}</p>
      <button
        onClick={reset}
        className="rounded-md bg-primary px-4 py-2 text-sm text-primary-foreground"
      >
        다시 시도
      </button>
    </div>
  );
}
```
