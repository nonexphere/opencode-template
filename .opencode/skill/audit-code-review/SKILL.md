---
name: audit-code-review
version: 1.1.0
category: fullstack
complexity: high
estimated_time: "30-60m"
chains_to:
  - create-api
  - os-architect
  - data-architect
  - perf-optimizer
  - audit-ui-vibeos
---

# Skill: Audit Code Review

> Comprehensive multi-phase code audit to identify technical debt, architectural flaws, security vulnerabilities, and refactoring opportunities.

---

## Purpose

Perform a comprehensive multi-phase analysis of a system, module, or codebase. The goal is to identify **ALL** technical debts, architectural flaws, security vulnerabilities, and code quality issues. The final output is a structured report and inline annotations that enable informed refactoring.

**Use when**:
- Auditing existing codebase before refactoring
- Evaluating technical debt
- Security review of a module
- Pre-release quality check
- Onboarding to understand a codebase

---

## Approaches

### Quick (15 min)
- Focus on CRITICAL and HIGH severity only
- Skip detailed testing audit
- Document only major findings
- No inline TODO markers

### Complete (30-60 min)
Full multi-phase audit:
1. Discovery & Mapping (PLANNING)
2. Code Quality Scan (EXECUTION)
3. Reporting (VERIFICATION)

### Incremental (multiple sessions)
For large codebases:
1. Session 1: Discovery + Structure mapping
2. Session 2: Security + Architecture review
3. Session 3: Performance + Testing review
4. Session 4: Final report + Recommendations

---

## Main Prompt

```
Perform a **comprehensive, multi-phase audit** of the target system. The goal is to identify **ALL** forms of technical debt, architectural flaws, security vulnerabilities, and code quality issues. The final output should be a structured report and inline code annotations that enable a subsequent **complete and informed refactoring**.

## Target System
[Specify the path or module to audit]

## Optional Focus
[Specify a primary focus if desired: "Security", "Performance", "API Design", "DRY/Deduplication"]

---

## Phase 1: Discovery & Mapping (PLANNING Mode)
1.  **Map File Structure**: List all directories and files. Identify entry points, core logic, utilities, types, and tests.
2.  **Identify Dependencies**: List internal and external dependencies. Note version inconsistencies.
3.  **Understand Data Flow**: Trace data from entry points through services to database.
4.  **Document in task.md**.

---

## Phase 2: Code Quality Scan (EXECUTION Mode)

### A. Typing & Safety
-   [ ] Scan for `: any`, `as any`
-   [ ] Check `catch(error)` blocks for proper type narrowing

### B. Logging & Observability
-   [ ] Find all `console.log`, flag for migration to logger
-   [ ] Scan for empty catch blocks

### C. Architecture & Design
-   [ ] Check separation of concerns (routes thin, services have logic)
-   [ ] Verify repository pattern usage
-   [ ] Scan for layering violations
-   [ ] Identify logic duplication (DRY violations)

### D. Security
-   [ ] Path Traversal in file handling
-   [ ] Input validation with Zod
-   [ ] SQL injection prevention
-   [ ] Hardcoded secrets

### E. Testing
-   [ ] Check for `__tests__` directories
-   [ ] Note files without tests

---

## Phase 3: Reporting (VERIFICATION Mode)
1.  **Generate DEBT_REPORT.md**: Categorize by severity (CRITICAL, HIGH, MEDIUM, LOW)
2.  **Add Inline Annotations (SOP-002)**:
    -   `// @BUG: [description]`
    -   `// @VULN: [description]`
    -   `// @SECURITY: [description]`
    -   `// @TECH-DEBT: [description]`
    -   `// @PERF: [description]`
    -   `// @TODO: [description]`
3.  **Generate Refactoring Recommendations**

---

## Expected Deliverables
1.  Updated task.md with audit phases
2.  DEBT_REPORT.md artifact
3.  Inline annotations in source files (SOP-002)
4.  walkthrough.md summary

---

## Constraints
-   **Do NOT auto-fix issues** unless trivially safe
-   **Prioritize findings** ruthlessly
-   **Be verbose in reports** but **concise in code comments**
```

---

## Severity Levels

| Level | Symbol | Priority | Examples |
|-------|--------|----------|----------|
| CRITICAL | RED | Must fix immediately | Security vulnerabilities, data loss risks |
| HIGH | ORANGE | Fix before next release | Broken features, performance issues |
| MEDIUM | YELLOW | Fix when convenient | Code quality, minor improvements |
| LOW | GREEN | Nice to have | Style, optional enhancements |

---

## Audit Checklist

### Security Red Flags
- `eval()`, `innerHTML`, `dangerouslySetInnerHTML`
- Hardcoded passwords, API keys, tokens
- Missing input validation
- Unparameterized SQL queries
- Path traversal vulnerabilities

### Architecture Red Flags
- Fat routes with business logic
- Missing service layer
- Direct DB access from handlers
- Circular dependencies
- God objects

### Code Quality Red Flags
- Functions > 100 lines
- Deep nesting (> 4 levels)
- Magic numbers without constants
- Commented-out code
- `any` types without justification

---

## Domain-Specific Chaining

### If auditing Backend (`backend/`)
| Next Action | Skill | Trigger |
|-------------|-------|---------|
| Create/refactor API | `create-api` | API issues found |
| Schema changes needed | `migrate-database` | DB issues found |
| Data architecture issues | `data-architect` | Data patterns broken |
| Performance issues | `perf-optimizer` | Perf debt identified |

### If auditing VibeOS/Frontend (`vibeos-react/`)
| Next Action | Skill | Trigger |
|-------------|-------|---------|
| UI-specific audit | `audit-ui-vibeos` | React component issues |
| Create new app | `create-app-vibeos` | App refactor needed |
| OS architecture issues | `os-architect` | System-level problems |
| Performance issues | `perf-optimizer` | Render perf debt |

---

## Skill Chaining

### Chains To
- **create-api**: When API refactoring is necessary
  - Trigger: DEBT_REPORT contains CRITICAL SECURITY issues
  - Input: Security findings

- **os-architect / data-architect**: For architectural issues
  - Trigger: DEBT_REPORT contains CRITICAL ARCHITECTURE issues
  - Input: Architecture findings

- **perf-optimizer**: For performance issues
  - Trigger: DEBT_REPORT contains HIGH PERFORMANCE issues
  - Input: Performance findings

- **audit-ui-vibeos**: When auditing frontend code
  - Trigger: Target is in `vibeos-react/`
  - Input: Module path

### Can Be Chained From
- Any implementation task for validation
- **codebase-analysis**: After initial understanding

---

## Evolution

### v1.1.0 (2026-01-07)
- Integrated SOP-002: Inline Audit Protocol
- Standardized tags (@BUG, @VULN, @SECURITY, @TECH-DEBT, @PERF, @TODO)
- Added terminal search instructions

### v1.0.0 (2026-01-06)
- Migrated from .skills/ to .opencode/skill/
- Added approaches section
- Added domain-specific chaining
- Enhanced severity classification
