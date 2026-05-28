# Step 9: Complete Route Handler Template

Full boilerplate for a standard CRUD route:

```typescript
// app/api/organizations/[orgId]/projects/route.ts
import { NextRequest, NextResponse } from 'next/server';
import { z } from 'zod';
import { getAuthContext, verifyOrgAccess } from '@/lib/auth';
import { apiSuccess, apiError } from '@/lib/api-response';
import { db } from '@/lib/db';

// --- Validation Schemas ---
const createProjectSchema = z.object({
  name: z.string().min(1).max(100),
  description: z.string().max(500).optional(),
});

const listQuerySchema = z.object({
  page: z.coerce.number().int().min(1).default(1),
  limit: z.coerce.number().int().min(1).max(100).default(20),
  status: z.enum(['draft', 'active', 'archived']).optional(),
});

// --- GET /api/organizations/[orgId]/projects ---
export async function GET(
  req: NextRequest,
  { params }: { params: { orgId: string } }
) {
  try {
    const auth = await getAuthContext(req);
    if (!auth) return apiError('Authentication required', 401);

    const access = await verifyOrgAccess(auth.userId, params.orgId);
    if (!access) return apiError('Access denied', 403);

    const query = listQuerySchema.safeParse(
      Object.fromEntries(new URL(req.url).searchParams)
    );
    if (!query.success) return apiError('Invalid query', 422, query.error.flatten());

    const { page, limit, status } = query.data;
    const offset = (page - 1) * limit;

    const [projects, total] = await Promise.all([
      db.projects.list({ orgId: params.orgId, status, limit, offset }),
      db.projects.count({ orgId: params.orgId, status }),
    ]);

    return NextResponse.json({
      data: projects,
      meta: { total, page, limit },
    });
  } catch (error) {
    console.error('GET /projects failed:', error);
    return apiError('Internal server error', 500);
  }
}

// --- POST /api/organizations/[orgId]/projects ---
export async function POST(
  req: NextRequest,
  { params }: { params: { orgId: string } }
) {
  try {
    const auth = await getAuthContext(req);
    if (!auth) return apiError('Authentication required', 401);

    const access = await verifyOrgAccess(auth.userId, params.orgId);
    if (!access) return apiError('Access denied', 403);
    if (access.role === 'viewer') return apiError('Insufficient permissions', 403);

    const body = await req.json();
    const parsed = createProjectSchema.safeParse(body);
    if (!parsed.success) return apiError('Validation failed', 422, parsed.error.flatten());

    const project = await db.projects.create({
      ...parsed.data,
      organizationId: params.orgId,
      createdBy: auth.userId,
    });

    return apiSuccess(project, 201);
  } catch (error) {
    console.error('POST /projects failed:', error);
    return apiError('Internal server error', 500);
  }
}
```

**Output summary:**

```
 API Designer — Routes Complete

Feature: [name]
Routes: [count] endpoints across [count] resource groups

Route Table:
  Method  Path                                Auth   Status
  GET     /api/organizations                       List
  POST    /api/organizations                       Create
  GET     /api/organizations/[orgId]           +O  Detail
  ...

Files to create:
  - app/api/organizations/route.ts
  - app/api/organizations/[orgId]/route.ts
  - app/api/organizations/[orgId]/projects/route.ts
  - lib/validations/organizations.ts (Zod schemas)
  - types/api/organizations.ts (TypeScript interfaces)

Zod schemas: [count] request validators
Type definitions: [count] interfaces
Webhooks: [count] with signature verification
Rate-limited routes: [count]
```
