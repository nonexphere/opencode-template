# NexusOS Monorepo Governance & Structure

<!-- @META: Monorepo Governance -->
<!--
    File: .opencode/specs/architecture/MONOREPO_GOVERNANCE.md
    Version: 2.0.0
    Created: 2026-01-07
    Updated: 2026-01-07
    Scope: Governance, Architecture, and Policy for the entire repository
    Status: Living Document
-->

---

## 1. Monorepo Philosophy

<!-- @NOTE(phil-001): Core Principles -->
<!-- @REF(AGENTS.md#universal-rules): Aligned with master constitution -->

NexusOS adopts a **Unified Monorepo Strategy** designed to maximize code sharing while maintaining strict modular boundaries. Unlike traditional "polyrepos" where context is fragmented, our structure allows for:

1.  **Atomic Changes**: A single PR can update a core library (e.g., `backend/packages/types`) and all its consumers (Frontend, Backend, AI) simultaneously.
2.  **Shared Intelligence**: AI Agents (`.opencode/`) have visibility over the entire stack, enabling cross-domain reasoning (e.g., "How does a database schema change impact the UI?").
3.  **Unified Tooling**: A single `pnpm` lockfile and `turbo` pipeline ensures deterministic builds across the entire operating system.

### The "Nexus" Trade-off
We accept **higher initial complexity** (workspace management, build pipelines) in exchange for **long-term velocity** and **consistency**.

---

## 2. Governance & Ownership

<!-- @NOTE(own-001): Ownership Matrix -->
<!-- @RULE: CODEOWNERS compliance is mandatory for critical paths -->

Ownership is distributed by **Domain** rather than by language.

| Domain | Scope (Glob Pattern) | Primary Owner | Team | Criticality |
|--------|----------------------|---------------|------|-------------|
| **Platform** | `/.opencode/**/*`, `/infrastructure/**/*` | @Leonidas | Platform Ops | 🔴 Critical |
| **Frontend** | `/vibeos-react/**/*` | @ReactArchitect | UI/UX Core | 🟠 High |
| **Backend** | `/backend/**/*` | @BackendLead | Cloud Svcs | 🟠 High |
| **AI/Neural** | `/neural/**/*` | @AIOrchestrator | AI Research | 🟡 Medium |
| **Shared** | `/**/packages/types/**/*` | @SystemArchitect | API Design | 🔴 Critical |
| **Documentation** | `/**/AGENTS.md`, `/docs/**/*` | @DocKeeper | DevRel | 🟢 Low |

### Ownership Responsibilities
- **Architects**: Define interfaces (`.d.ts`), approve schema changes, and manage `AGENTS.md`.
- **Maintainers**: Fix bugs, upgrade dependencies, and ensure test coverage.
- **Contributors**: Submit PRs adhering to the governance model.

---

## 3. Dependency Management

<!-- @RULE: strict-pnpm -->
<!-- @SOURCE(pnpm-workspace.yaml): Workspace enforcement -->

We use **pnpm workspaces** to manage dependencies. This is non-negotiable.

### Policy 3.1: Internal Dependencies
Internal packages MUST be referenced using the `workspace:*` protocol to ensure the latest local version is always used during development.

**Correct:**
```json
{
  "dependencies": {
    "@nexus/types": "workspace:*"
  }
}
```

**Incorrect:**
```json
{
  "dependencies": {
    "@nexus/types": "1.0.0" // ❌ Risks version drift
  }
}
```

### Policy 3.2: External Dependencies
- **Peer Dependencies**: Shared libraries (`react`, `lodash`) should be peer dependencies in library packages to avoid duplication.
- **Version Pinning**: All production dependencies must use exact versions or carets (`^`) with strict lockfile checks.
- **Vendoring**: Third-party code that requires heavy modification MUST be placed in `vendors/` and documented with `README.vendor.md`.

---

## 4. Build System & CI Pipelines

<!-- @NOTE(build-001): Toolchain -->
<!-- @REF(backend/package.json): Turbo configuration -->

The build system is a hybrid model optimized for different execution environments.

### 4.1 Global Orchestration (Turborepo)
The `backend` and root scripts utilize **Turborepo** for task orchestration.
- **Caching**: Inputs/Outputs are hashed. If code hasn't changed, the task is replayed from cache (`.turbo/`).
- **Pipeline**:
    1.  `build` depends on `^build` (topological sort).
    2.  `test` depends on `build`.
    3.  `lint` runs in parallel.

### 4.2 Frontend Optimization (Vite)
`vibeos-react` uses **Vite** for sub-millisecond HMR (Hot Module Replacement).
- **SWC/Esbuild**: Used for transpilation.
- **Chunking**: Manual chunk splitting defined in `vite.config.ts` for optimal loading.

### 4.3 Mobile Build Chain (Capacitor)
Mobile builds follow a strict sequence:
1.  `pnpm build` (React build)
2.  `cap sync` (Copy assets to native platforms)
3.  `cap open` (Launch IDE)

---

## 5. Package Lifecycle & Standards

<!-- @NOTE(lifecycle-001): CRUD for Packages -->

### 5.1 Creating a New Package
1.  **Identify Scope**: Is it Backend (`backend/packages`), Frontend (`vibeos-react/packages`), or Universal (`packages/`)?
2.  **Scaffold**: Create directory structure (`src/`, `tsconfig.json`, `package.json`).
3.  **Document**: Create `AGENTS.md` (Mandatory).
4.  **Register**: Add to `pnpm-workspace.yaml` if not covered by wildcards.

### 5.2 Deprecation Process
1.  Add `<!-- @DEPRECATED: [Reason] -->` to the package's `AGENTS.md`.
2.  Add `console.warn` in the entry point.
3.  Move to `deprecated/` folder (optional) or mark as `status: deprecated` in metadata.

### 5.3 Naming Conventions
- **Apps**: `kebab-case` (e.g., `billing-gateway`).
- **Packages**: Scoped `@nexus/[name]` or `@nexus-os/[name]`.
- **Classes**: `PascalCase`.
- **Files**: `camelCase.ts` (logic) or `PascalCase.tsx` (components).

---

## 6. Health Metrics & Inventory

<!-- @NOTE(health-001): Living Status Report -->
<!-- @DATA: Estimated Debt Scores based on complexity and test presence -->

### 🔴 Backend Core (Infrastructure)
| Package | Status | Owner | Test Cov (Est) | Debt Score | Last Audit |
|---------|--------|-------|----------------|------------|------------|
| `backend/hub` | ✅ Active | @BackendLead | High | 🟢 Low | 2025-12 |
| `packages/db` | ✅ Active | @DataArch | Medium | 🟡 Med | 2025-11 |
| `packages/circuit-breaker` | ✅ Active | @BackendLead | High | 🟢 Low | 2025-10 |
| `packages/worker` | ⚠️ Beta | @BackendLead | Low | 🟠 High | 2026-01 |
| `packages/logger` | ✅ Active | @Platform | High | 🟢 Low | 2025-12 |
| `packages/events` | ✅ Active | @Platform | Medium | 🟢 Low | 2025-12 |

### 🟠 Backend Apps (Microservices)
| App | Status | Owner | Test Cov (Est) | Debt Score | Complexity |
|-----|--------|-------|----------------|------------|------------|
| `billing-gateway` | ⚠️ Beta | @Commerce | Low | 🟠 High | High |
| `ai-inference` | 🚧 Alpha | @AIOrchestrator| N/A | 🟡 Med | Very High |
| `cloud-storage` | ✅ Active | @Infra | Medium | 🟢 Low | Medium |
| `knowledge-base` | 🚧 Alpha | @DataArch | Low | 🟡 Med | High |

### 🔵 Frontend Packages (VibeOS Apps)
| Package | Status | Owner | Framework | Debt Score | Notes |
|---------|--------|-------|-----------|------------|-------|
| `vibeos-react` | ✅ Active | @ReactArch | React 19 | 🟡 Med | Monolith |
| `packages/roo-code` | ✅ Active | @AITeam | React | 🟢 Low | AI Assistant |
| `packages/neon-snake` | 💤 Maint | @GameDev | React | 🟢 Low | Demo App |
| `packages/retro-stream`| ⚠️ Beta | @MediaTeam | React | 🟠 High | AV Logic |
| `packages/ghost-protocol`| 🚧 Alpha | @Security | React | 🔴 Crit | Needs Review |

### 🟣 AI & Orchestration
| Package | Status | Owner | Purpose | Debt Score |
|---------|--------|-------|---------|------------|
| `neural/` | 🚧 Alpha | @Leonidas | CI/CD Agent | 🟡 Med |
| `.opencode/` | ✅ Active | @Leonidas | Agent OS | 🟢 Low |

---

## 7. Migration Strategies

<!-- @NOTE(mig-001): Moving code -->

### Breaking the Monolith
When a module in `vibeos-react/system42` becomes too large or is needed by the backend:
1.  **Extract**: Move code to `packages/[name]`.
2.  **Abstract**: Remove React-specific dependencies (use Adapters).
3.  **Publish**: Add `package.json` with `@nexus/[name]`.
4.  **Consume**: Update `vibeos-react` to import from the new package.

### Legacy Services to System52
1.  Create Interface in `system52/interfaces`.
2.  Implement Service in `system52/services`.
3.  Create `AGENTS.md` in `system52/`.
4.  Refactor consumers to use `SystemBus` instead of direct imports.

---

## 8. Performance Budgets

<!-- @RULE: perf-limits -->
<!-- @WHY: Prevent bloat on low-end devices -->

| Metric | Target | Warning | Critical | Action |
|--------|--------|---------|----------|--------|
| **Initial Bundle (JS)** | < 150KB | > 200KB | > 300KB | Split chunks, lazy load |
| **First Contentful Paint** | < 1.0s | > 1.5s | > 2.5s | Optimize assets |
| **Backend Cold Start** | < 500ms | > 1s | > 3s | Optimize imports |
| **Docker Image Size** | < 200MB | > 500MB | > 1GB | Use Alpine/Distroless |

---

## 9. Inter-Package Communication

<!-- @NOTE(ipc-001): Contracts -->

### 9.1 Shared Types
All DTOs (Data Transfer Objects) MUST be defined in `backend/packages/types` (or `packages/shared`).
- **No duplication**: Frontend and Backend must import the same interface.
- **Zod Validation**: Use `zod` schemas co-located with types for runtime validation.

### 9.2 API Contracts
- **REST**: OpenAPI (Swagger) generated from Zod schemas.
- **WebSocket**: Event types defined in `@nexus/types/events`.
- **SystemBus**: Local events defined in `vibeos-react/system42/systemBus.ts`.

---

## 10. Workspace Structure Reference

<!-- @REF(tree-view): Visual map -->

```text
nexus-os-dash/
├── pnpm-workspace.yaml      # 🗺️ The Map
├── package.json             # 🎼 The Conductor (Scripts)
├── turbo.json               # 🚀 The Engine (Cache)
├── AGENTS.md                # 📜 The Law
│
├── .opencode/               # 🧠 The Brain
│   ├── docs/                # Knowledge Base
│   └── context/             # Session Memory
│
├── vibeos-react/            # 🖥️ The Face (Frontend)
│   ├── src/
│   └── packages/            # Local Apps
│
├── backend/                 # ⚙️ The Machinery
│   ├── hub/                 # Signaling
│   ├── packages/            # Shared Libs
│   └── apps/                # Microservices
│
└── neural/                  # 👁️ The Observer (AI)
```

---

## 11. Testing Strategy & Quality Assurance

<!-- @NOTE(qa-001): Testing Pyramid -->
<!-- @REF(AGENTS.md#testing-policy): Co-location rule -->

### 11.1 The Testing Pyramid
We strictly adhere to the testing pyramid to ensure fast feedback loops.

| Type | Scope | Tool | Location | Target Cov |
|------|-------|------|----------|------------|
| **Unit** | Individual functions/components | `vitest` | `src/__tests__` or `*.test.ts` | 80% |
| **Integration** | Service interaction, database | `vitest` + `testcontainers` | `test/integration` | 60% |
| **E2E** | Full user flows | `playwright` | `e2e/` | Critical Flows |

### 11.2 Testing Rules
1.  **Co-location**: Unit tests MUST sit next to the file they test (e.g., `Button.tsx` -> `Button.test.tsx`).
2.  **Mocking Boundaries**:
    - **Frontend**: Mock all network requests (MSW).
    - **Backend**: Mock external APIs, but use real DB in Docker for integration tests.
3.  **Snapshot Testing**: Use sparingly. Prefer assertion on logic over markup.

---

## 12. Contribution & Review Guidelines

<!-- @NOTE(contrib-001): Human-AI Collaboration -->

### 12.1 Pull Request Standards
All PRs must include:
- [ ] Reference to the Task ID (e.g., `[TASK-123]`).
- [ ] `AGENTS.md` updates if architecture changed.
- [ ] Test coverage report.
- [ ] Before/After screenshots (for UI).

### 12.2 Code Review Checklist
Reviewers (Human or AI) check for:
1.  **Security**: Are inputs sanitized? Are permissions checked?
2.  **Performance**: Any new N+1 queries? Large bundles?
3.  **Maintainability**: Is code readable? Are names descriptive?
4.  **Governance**: Does it respect the package boundaries?

### 12.3 Branching Strategy
- `main`: Production-ready. Protected.
- `dev`: Integration branch.
- `feat/name`: Feature branches.
- `fix/name`: Bug fixes.

---

## 13. Security Policy

<!-- @RULE: sec-001 -->

1.  **Secret Zero**: No `.env` files in git.
2.  **Audit**: Run `pnpm audit` weekly.
3.  **Dependencies**: No abandoned packages (< 1 year updates).
4.  **Sanitization**: All inputs in `backend/apps` must be sanitized via `zod`.

---

<!-- @DEPRECATED: Old Structures -->
<!--
The following directories are slated for removal in v3.0.0:
- `services/` (Legacy in vibeos-react, move to system52)
- `vendors/old-lib` (Replace with npm equivalent)
-->
