---
name: audit-ui-vibeos
version: 1.0.0
category: frontend
complexity: high
estimated_time: "30-60m"
chains_to:
  - perf-optimizer
  - os-architect
  - create-app-vibeos
---

# Skill: Audit UI (VibeOS)

> Comprehensive UI/UX audit of VibeOS React components focusing on accessibility, performance, design system compliance, and code quality.

---

## Purpose

Perform a deep analysis of VibeOS React components, identifying accessibility issues, performance bottlenecks, design inconsistencies, and code quality problems.

**Use when**:
- Auditing React components for accessibility
- Performance review of UI components
- Design system compliance check
- Pre-release UI quality validation
- Component refactoring planning

---

## Approaches

### Quick (15 min)
- Focus on accessibility and critical performance issues
- Skip detailed design system audit
- Document only major findings

### Complete (30-60 min)
Full 6-phase audit:
1. Component Mapping (PLANNING)
2. Accessibility Audit (EXECUTION)
3. Performance Audit (EXECUTION)
4. Design System Compliance (EXECUTION)
5. Code Quality (EXECUTION)
6. Reporting (VERIFICATION)

### Incremental (multiple sessions)
For large apps/component libraries:
1. Session 1: Mapping + Accessibility
2. Session 2: Performance
3. Session 3: Design System + Code Quality
4. Session 4: Final report

---

## Main Prompt

```
Perform a **comprehensive UI/UX audit** of a VibeOS React component or application.

## Target
[Specify component/app path, e.g., `apps/settings/SettingsApp.tsx`]

## Audit Scope
[Select: FULL_APP | SINGLE_COMPONENT | COMPONENT_FAMILY]

---

## Phase 1: Component Mapping (PLANNING Mode)
1.  **List All Components**: Identify component tree
2.  **Identify Prop Interfaces**: Document props and types
3.  **Map State Management**: useState, useReducer, Context
4.  **Check Test Coverage**: Existing tests?
5.  **Document in task.md**

---

## Phase 2: Accessibility Audit (EXECUTION Mode)

### A. Semantic HTML
-   [ ] Correct heading hierarchy
-   [ ] Semantic elements (nav, main, section)
-   [ ] No div soup

### B. ARIA Compliance
-   [ ] Interactive elements have accessible names
-   [ ] Dynamic content announces (aria-live)
-   [ ] Modal dialogs trap focus
-   [ ] Custom components have correct roles

### C. Keyboard Navigation
-   [ ] All interactive elements focusable
-   [ ] Logical focus order
-   [ ] Visible focus styles
-   [ ] Escape closes modals

### D. Color & Contrast
-   [ ] Sufficient contrast ratio (4.5:1 for text)
-   [ ] Info not conveyed by color alone

---

## Phase 3: Performance Audit (EXECUTION Mode)

### A. Render Optimization
-   [ ] Large lists virtualized
-   [ ] useMemo for expensive computations
-   [ ] useCallback for callbacks passed to children
-   [ ] No inline object literals in JSX

### B. Code Splitting
-   [ ] React.lazy + Suspense for large components
-   [ ] Route-based splitting

### C. Memory Leaks
-   [ ] useEffect cleanup functions
-   [ ] Event listeners removed
-   [ ] Subscriptions unsubscribed

---

## Phase 4: Design System Compliance (EXECUTION Mode)

### A. Token Usage
-   [ ] CSS variables for colors (--os-*)
-   [ ] Consistent spacing scale
-   [ ] Defined typography

### B. Component Consistency
-   [ ] Standard Button/Input components from UI library
-   [ ] Icon set consistency

### C. Responsive Design
-   [ ] Handles window resize
-   [ ] No overflow issues
-   [ ] Touch-friendly targets (44x44px minimum)

---

## Phase 5: Code Quality (EXECUTION Mode)

### A. TypeScript
-   [ ] No `any` types
-   [ ] Exported prop interfaces
-   [ ] Typed event handlers

### B. Hooks
-   [ ] Custom hooks for reusable logic
-   [ ] Correct dependency arrays
-   [ ] No conditional hooks

### C. Structure
-   [ ] Single-responsibility components
-   [ ] Logic separated from presentation

---

## Phase 6: Reporting (VERIFICATION Mode)
1.  **Generate Reports**: A11y, Performance, Design issues
2.  **Add Inline TODOs**:
    -   `// TODO(A11Y): [description]`
    -   `// TODO(PERF): [description]`
    -   `// TODO(UI): [description]`
3.  **Update DEBT_REPORT.md**

---

## Expected Deliverables
1.  Accessibility findings documented
2.  Performance issues identified
3.  Design inconsistencies listed
4.  Inline TODO markers
5.  Summary in walkthrough.md

---

## Constraints
-   **Do NOT refactor during audit**: Only document
-   **Prioritize by impact**: Accessibility > Performance > Design
-   **Test with keyboard only**
```

---

## Audit Checklists

### Accessibility Red Flags
- Missing `alt` attributes on images
- Non-semantic click handlers (`div onClick`)
- Missing focus management in modals
- Missing skip links
- No visible focus indicators

### Performance Red Flags
- Inline functions in JSX props
- Missing keys or unstable keys in lists
- Large component re-renders on every state change
- No virtualization for long lists
- Missing React.memo on expensive pure components

### Design System Red Flags
- Hardcoded colors instead of CSS variables
- Inconsistent spacing (px instead of scale)
- Mixed icon libraries
- Custom implementations of standard components

---

## Skill Chaining

### Can Be Called By
- **audit-code-review**: When auditing code in `vibeos-react/`

### Chains To
- **perf-optimizer**: For performance issues
  - Trigger: DEBT_REPORT contains HIGH PERFORMANCE issues
  - Input: Performance findings

- **os-architect**: For architecture issues
  - Trigger: DEBT_REPORT contains CRITICAL ARCHITECTURE issues
  - Input: System-level problems

- **create-app-vibeos**: When refactoring requires new component
  - Trigger: Component needs complete rewrite
  - Input: Component requirements

---

## Evolution

### v1.0.0 (2026-01-06)
- Migrated from .skills/ to .opencode/skill/
- Added approaches section
- Added skill chaining
- Enhanced with red flag checklists
