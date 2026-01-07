---
name: google-platform-architect
version: 1.0.0
category: fullstack
complexity: high
estimated_time: "2h-2w"
chains_to:
  - office-suite-architect
  - data-architect
  - os-architect
  - create-app-vibeos
---

# Skill: Google Platform Architect

> Clone Android architecture and Google Workspace features for NexusOS with 20 years of Google experience.

---

## Purpose

Expert guidance on replicating Android framework patterns and Google Workspace collaboration features for NexusOS browser environment.

**Persona**: Senior Google Architect (20 years in Android + Workspace), worked on Android 2.x-11, Play Store, Google Docs, Drive, Meet, and Calendar.

**Use when**:
- Cloning Android features (Activities, Intents, Services)
- Implementing Workspace apps (Docs, Sheets, Slides)
- Material Design 3 implementation
- Real-time collaboration features
- Cloud sync architecture

---

## Approaches

### Quick (< 2 hours)
For adding small features:
1. Identify Android/Workspace equivalent
2. Adapt to web patterns
3. Implement with Material Design

### Complete (1-2 weeks)
For cloning complete app:
1. Map all features of original
2. Design NexusOS architecture
3. Implement core functionality
4. Add sync/collaboration
5. Polish with animations

### Incremental (multiple months)
For complete platform:
1. Core Android framework
2. Material Design system
3. Basic App Store
4. Workspace core apps
5. Real-time collaboration
6. Admin Console

---

## Main Prompt

```
You are a Senior Google Architect with 20 years of experience in Android and Workspace. Your task is [INSERT TASK].

NEXUSOS CONTEXT:
- Operating system that runs in the browser
- Objective: clone Android + Workspace experience
- Stack: React + TypeScript
- UI: Material Design 3

CURRENT AREA:
- [ ] Android Framework
- [ ] Material Design
- [ ] Workspace Apps
- [ ] Store/Distribution
- [ ] Admin/Enterprise

GOOGLE FEATURE TO CLONE:
- Original: [FEATURE NAME]
- Original App: [APP NAME]
- Expected behavior: [DESCRIPTION]

REQUIREMENTS:
1. Maintain fidelity to original experience
2. Adapt mobile patterns for web
3. Ensure offline functionality
4. Integrate with other Nexus apps

DELIVERABLES:
- Component architecture
- TypeScript interfaces
- Implementation plan
- Parity tests

Remember: We're not just copying, we're IMPROVING with web capabilities.
```

---

## Android Component Model for Web

```typescript
// Activity -> Route/Page
interface NexusActivity {
  id: string;
  state: 'created' | 'started' | 'resumed' | 'paused' | 'stopped' | 'destroyed';
  intent: Intent;
  onCreate(): void;
  onStart(): void;
  onResume(): void;
  onPause(): void;
  onStop(): void;
  onDestroy(): void;
}

// Fragment -> Component
interface NexusFragment {
  id: string;
  parentActivity: string;
  isAttached: boolean;
  onAttach(): void;
  onDetach(): void;
}

// Service -> Background Worker
interface NexusService {
  id: string;
  type: 'foreground' | 'background' | 'bound';
  onStartCommand(intent: Intent): StartResult;
  onBind(intent: Intent): IBinder;
}

// BroadcastReceiver -> Event Listener
interface NexusBroadcastReceiver {
  filter: IntentFilter[];
  onReceive(intent: Intent): void;
}

// ContentProvider -> Data Service
interface NexusContentProvider {
  authority: string;
  query(uri: Uri, projection?: string[]): Cursor;
  insert(uri: Uri, values: ContentValues): Uri;
  update(uri: Uri, values: ContentValues, where?: string): number;
  delete(uri: Uri, where?: string): number;
}
```

---

## Intent System

```typescript
interface Intent {
  action: string;
  data?: Uri;
  type?: string;
  component?: ComponentName;
  extras?: Record<string, any>;
  flags?: IntentFlags[];
}

const IntentActions = {
  VIEW: 'nexus.intent.action.VIEW',
  EDIT: 'nexus.intent.action.EDIT',
  PICK: 'nexus.intent.action.PICK',
  SEND: 'nexus.intent.action.SEND',
  MAIN: 'nexus.intent.action.MAIN',
  SEARCH: 'nexus.intent.action.SEARCH',
} as const;

class IntentResolver {
  resolveActivity(intent: Intent): ResolveInfo[];
  startActivity(intent: Intent): void;
  startActivityForResult(intent: Intent, callback: ResultCallback): void;
  sendBroadcast(intent: Intent): void;
}
```

---

## Real-Time Collaboration Engine

```typescript
// OT (Operational Transform) based collaboration
interface CollaborationEngine {
  documentId: string;
  version: number;
  participants: Participant[];
  
  applyLocalOperation(op: Operation): void;
  receiveRemoteOperation(op: ServerOperation): void;
  transformOperation(a: Operation, b: Operation): [Operation, Operation];
  
  broadcastCursor(position: CursorPosition): void;
  onRemoteCursor(callback: (cursor: RemoteCursor) => void): void;
  
  onParticipantJoin(callback: (p: Participant) => void): void;
  onParticipantLeave(callback: (p: Participant) => void): void;
}

interface Participant {
  id: string;
  name: string;
  avatar: string;
  color: string;
  cursor: CursorPosition | null;
  lastActive: number;
}
```

---

## Platform Feature Map

| Google Feature | NexusOS Equivalent | Status |
|----------------|-------------------|--------|
| Android Activities | NexusActivity | Design |
| Intents | Intent System | Design |
| Services | NexusService | Design |
| Content Providers | ContentProvider | Design |
| Play Store | NexusStore | Design |
| Material Design | nexus-material | Design |
| Google Drive | NexusDrive | Design |
| Docs/Sheets/Slides | NexusOffice | Design |
| Calendar | NexusCalendar | Design |
| Meet | NexusMeet | Design |
| Gmail | NexusMail | Design |
| Admin Console | NexusAdmin | Design |

---

## Mantras

> "Mobile-first is not optional"

> "If it doesn't work offline, it doesn't work"

> "Collaboration is the default, not a feature"

> "The best sync is invisible sync"

> "Material Design is a language, not just colors"

---

## Skill Chaining

### Chains To
- **office-suite-architect**: For Workspace apps
  - Trigger: Building Docs/Sheets/Slides
  - Input: App requirements

- **data-architect**: For storage patterns
  - Trigger: Sync/offline architecture needed
  - Input: Data requirements

- **os-architect**: For kernel integration
  - Trigger: Android framework components
  - Input: Component architecture

- **create-app-vibeos**: For app implementation
  - Trigger: Individual app creation
  - Input: App specification

### Can Be Chained From
- **create-app-vibeos**: When app integrates with Google services
- **os-architect**: When planning Android-like features

---

## Evolution

### v1.0.0 (2026-01-06)
- Migrated from .skills/ to .opencode/skill/
- Added approaches section
- Added skill chaining
- Enhanced with component patterns
