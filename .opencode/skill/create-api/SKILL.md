---
name: create-api
version: 1.0.0
category: backend
complexity: medium
estimated_time: "30-60m"
chains_to:
  - migrate-database
  - audit-code-review
---

# Skill: Create API

> Design and implement RESTful APIs following clean architecture patterns and conventions.

---

## Purpose

Guide the complete process of designing and implementing a RESTful API, from endpoint definition to tests and documentation. Follows clean architectural patterns (Routes -> Services -> Repositories).

**Use when**:
- Creating new API endpoints
- Implementing CRUD for a resource
- Adding new module to the backend
- Refactoring existing API

---

## Approaches

### Quick (15 min)
For simple single-endpoint additions:
1. Add Zod schema
2. Implement route handler
3. Call existing service
4. Basic test

### Complete (30-60 min)
Full API implementation:
1. Requirements & Data Modeling (PLANNING)
2. Schema & Validation (EXECUTION)
3. Route Implementation (EXECUTION)
4. Service & Repository (EXECUTION)
5. Testing (VERIFICATION)

### Incremental (multiple sessions)
For complex modules:
1. Session 1: Design + Schemas
2. Session 2: Routes + Services
3. Session 3: Repository + Tests
4. Session 4: Documentation + Review

---

## Main Prompt

```
Perform a **comprehensive API design and implementation** for a new or existing endpoint/module in your backend.

## Target Module
[Specify the module path, e.g., `src/modules/notifications`]

## API Scope
[Describe the resource and actions, e.g., "CRUD for user notifications"]

---

## Phase 1: Requirements & Data Modeling (PLANNING Mode)
1.  **Define the Resource**: What entity does this API manage?
2.  **List Operations**:
    -   [ ] `GET /resource` - List with pagination
    -   [ ] `GET /resource/:id` - Get single
    -   [ ] `POST /resource` - Create
    -   [ ] `PUT /resource/:id` - Update
    -   [ ] `DELETE /resource/:id` - Delete
    -   [ ] Custom actions
3.  **Define Data Shapes**: Request/response JSON structures
4.  **Identify Dependencies**: Auth, rate limits, related services
5.  **Document in implementation_plan.md**

---

## Phase 2: Schema & Validation (EXECUTION Mode)
1.  **Create Zod Schemas**:
    -   [ ] Request body schemas
    -   [ ] Query params schemas
    -   [ ] Response schemas
2.  **Export TypeScript Types**: Inferred from Zod
3.  **File**: `modules/[module]/[module].schemas.ts`

---

## Phase 3: Route Implementation (EXECUTION Mode)
1.  **Create Route Handler**: `modules/[module]/[module].routes.ts`
2.  **Follow Patterns**:
    -   [ ] Use `sendJson(res, status, data)`
    -   [ ] Parse body with Zod, return 400 on error
    -   [ ] Call service layer (never DB directly)
    -   [ ] Wrap in try/catch
3.  **Register in Router**

---

## Phase 4: Service & Repository (EXECUTION Mode)
1.  **Service**: `modules/[module]/[module].service.ts`
    -   Business logic and validation
    -   Error classes
2.  **Repository**: `modules/[module]/[module].repository.ts`
    -   Database queries with Drizzle
    -   No business logic

---

## Phase 5: Testing (VERIFICATION Mode)
1.  **Route Tests**: All HTTP methods, validation, auth, not found
2.  **Service Tests**: Business logic
3.  **JSDoc Comments**: Document all functions
4.  **Run**: `pnpm test --filter hub`

---

## Expected Deliverables
1.  Zod schemas
2.  Route handlers
3.  Service layer
4.  Repository layer
5.  Unit tests
6.  JSDoc documentation

---

## Constraints
-   **Follow Project Conventions**: Match existing `src/modules/` patterns
-   **Use structured logging**: No console.log
-   **Validate Everything**: All input through Zod
-   **Thin Routes**: Only parse, call services, respond
```

---

## Architecture Patterns

### File Structure
```
modules/[module]/
├── [module].routes.ts       # HTTP handlers
├── [module].service.ts      # Business logic
├── [module].repository.ts   # Database access
├── [module].schemas.ts      # Zod schemas + types
├── [module].errors.ts       # Custom error classes
└── __tests__/
    ├── [module].routes.test.ts
    └── [module].service.test.ts
```

### Route Pattern
```typescript
import { sendJson } from '@/utils/response';
import { MySchema } from './my.schemas';
import { myService } from './my.service';

export async function getMyResource(req: Request, res: Response) {
  try {
    const { id } = req.params;
    const result = await myService.getById(id);
    
    if (!result) {
      return sendJson(res, 404, { error: 'Not found' });
    }
    
    return sendJson(res, 200, result);
  } catch (error) {
    logger.error('Failed to get resource', { error });
    return sendJson(res, 500, { error: 'Internal error' });
  }
}
```

### Service Pattern
```typescript
export const myService = {
  async getById(id: string): Promise<MyResource | null> {
    return myRepository.findById(id);
  },
  
  async create(data: CreateMyResource): Promise<MyResource> {
    // Business logic, validation
    return myRepository.insert(data);
  },
};
```

---

## Skill Chaining

### Can Be Called By
- **audit-code-review**: When API refactoring is necessary

### Chains To
- **migrate-database**: When new entity is needed
  - Trigger: API requires new table
  - Input: Entity definition

- **audit-code-review**: After implementation for validation
  - Trigger: Implementation complete
  - Input: Module path

---

## Evolution

### v1.0.0 (2026-01-06)
- Migrated from .skills/ to .opencode/skill/
- Added approaches section
- Added skill chaining
- Enhanced with architecture patterns
