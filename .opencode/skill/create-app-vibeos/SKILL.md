---
name: create-app-vibeos
version: 1.0.0
category: frontend
complexity: medium
estimated_time: "30-60m"
chains_to:
  - audit-ui-vibeos
  - office-suite-architect
  - google-platform-architect
---

# Skill: Create App (VibeOS)

> Create new VibeOS applications following the operating system architecture and design patterns.

---

## Purpose

Guide the complete process of creating a new application for VibeOS, from scaffold structure to OS integration.

**Use when**:
- Creating a new VibeOS application
- Scaffolding app structure
- Integrating with OSContext
- Adding to system manifest

---

## Approaches

### Quick (15 min)
For minimal app scaffold:
1. Create folder structure
2. Basic [AppName]App.tsx component
3. Register in manifest
4. Verify app loads

### Complete (30-60 min)
Full app implementation:
1. Architecture Planning (PLANNING)
2. File Structure (EXECUTION)
3. Core Implementation (EXECUTION)
4. UI Implementation (EXECUTION)
5. Testing (VERIFICATION)

### Incremental (multiple sessions)
For complex apps:
1. Session 1: Planning + Structure
2. Session 2: Core logic + OSContext integration
3. Session 3: UI + Design System
4. Session 4: Tests + Polish

---

## Main Prompt

```
Perform a **complete scaffold and implementation** of a new VibeOS application.

## App Information
- **App Name**: [e.g., "Calculator", "Weather", "Notes"]
- **App ID**: [e.g., "calculator", "weather", "notes"]
- **App Type**: [SYSTEM_APP (system52/apps) | USER_APP (apps/)]
- **Category**: [Productivity | Utilities | Communication | Media | Developer | System]

---

## Phase 1: Architecture Planning (PLANNING Mode)
1.  **Define App Purpose**: What problem does it solve? Core features?
2.  **Identify Dependencies**:
    -   [ ] OS Services (OSContext, FileSystemService, etc.)
    -   [ ] External APIs
    -   [ ] Shared components (components/ui/)
3.  **Define State Structure**: What data does the app manage?
4.  **Sketch UI Layout**: Main views/panels
5.  **Document in implementation_plan.md**

---

## Phase 2: File Structure (EXECUTION Mode)
Create:
```
[apps|system52/apps]/[app-id]/
├── [AppName]App.tsx          # Main component
├── components/               # App-specific components
├── hooks/                    # Custom hooks
├── types.ts                  # App types
├── constants.ts              # Constants
├── __tests__/                # Tests
└── index.ts                  # Exports
```

---

## Phase 3: Core Implementation (EXECUTION Mode)

### A. Main Component
-   [ ] Functional component with hooks
-   [ ] Connect to OSContext
-   [ ] Loading and error states
-   [ ] Accessibility (ARIA, keyboard nav)

### B. App Registration
-   [ ] Add to manifest (systemApps or installedApps)
-   [ ] Define: id, name, icon, component, defaultSize, minSize

### C. State Management
-   [ ] useState/useReducer for local state
-   [ ] OSContext for shared state
-   [ ] Persist preferences if needed

---

## Phase 4: UI Implementation (EXECUTION Mode)
1.  **Design System**:
    -   [ ] CSS variables (--os-*)
    -   [ ] Consistent spacing/typography
    -   [ ] Dark/light mode
2.  **Responsive Layout**:
    -   [ ] Handle window resize
    -   [ ] Support min size constraints
3.  **Animations**:
    -   [ ] Subtle transitions
    -   [ ] Respect prefers-reduced-motion

---

## Phase 5: Testing (VERIFICATION Mode)
1.  **Unit Tests**:
    -   [ ] Component renders
    -   [ ] User interactions
    -   [ ] OS service integration (mocked)
2.  **Manual Testing**:
    -   [ ] Open/close via dock
    -   [ ] Resize, minimize, maximize
    -   [ ] Keyboard navigation

---

## Expected Deliverables
1.  Complete folder structure
2.  [AppName]App.tsx component
3.  App registered in manifest
4.  Basic unit tests
5.  App icon (or placeholder)
6.  JSDoc documentation

---

## Constraints
-   **No direct window management**: Use OSContext
-   **No global styles**: CSS modules or scoped styles
-   **No external state libraries**: React built-ins + OSContext
-   **Follow naming**: [AppName]App.tsx, use[Feature].ts
```

---

## App Structure Pattern

### Folder Structure
```
apps/[app-id]/
├── [AppName]App.tsx          # Main component
├── [AppName]App.module.css   # Scoped styles
├── components/
│   ├── Toolbar.tsx
│   └── ContentArea.tsx
├── hooks/
│   └── useAppState.ts
├── types.ts
├── constants.ts
├── index.ts
└── __tests__/
    └── [AppName]App.test.tsx
```

### Main Component Pattern
```typescript
import { useOSContext } from '@/context/OSContext';
import styles from './[AppName]App.module.css';

interface [AppName]AppProps {
  windowId: string;
}

export function [AppName]App({ windowId }: [AppName]AppProps) {
  const { fileSystem, notifications } = useOSContext();
  const [state, setState] = useState<AppState>(initialState);

  return (
    <div className={styles.container}>
      <Toolbar />
      <ContentArea state={state} />
    </div>
  );
}
```

### Manifest Registration
```typescript
// In manifest.ts or systemApps.ts
{
  id: 'app-id',
  name: 'App Name',
  icon: AppIcon,
  component: lazy(() => import('@/apps/app-id/AppNameApp')),
  defaultSize: { width: 800, height: 600 },
  minSize: { width: 400, height: 300 },
  category: 'productivity',
}
```

---

## Skill Chaining

### Can Be Called By
- **audit-ui-vibeos**: When refactoring requires new component
- **os-architect**: When planning new system apps

### Chains To
- **office-suite-architect**: If app is part of office suite
  - Trigger: App type is document/spreadsheet/presentation
  - Input: App requirements

- **google-platform-architect**: If app integrates with Google services
  - Trigger: App requires Drive/Calendar/Meet integration
  - Input: Integration requirements

- **audit-ui-vibeos**: After implementation for quality validation
  - Trigger: Implementation complete
  - Input: App path

---

## Evolution

### v1.0.0 (2026-01-06)
- Migrated from .skills/ to .opencode/skill/
- Added approaches section
- Added skill chaining
- Enhanced with patterns and examples
