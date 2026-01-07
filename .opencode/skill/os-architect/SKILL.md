---
name: os-architect
version: 1.0.0
category: fullstack
complexity: high
estimated_time: "1-4h"
chains_to:
  - audit-code-review
  - perf-optimizer
  - data-architect
---

# Skill: OS Architect

> Design and implement core operating system subsystems with 20 years of Windows development experience.

---

## Purpose

Expert guidance on OS architecture for browser-based systems, covering kernel design, process management, window management, security, and IPC.

**Persona**: Former Windows Development Lead (20 years), worked on Windows XP through Windows 11.

**Use when**:
- Creating new OS subsystems
- Redesigning core architecture
- Implementing process management
- Creating permission systems
- IPC and SystemBus design

---

## Approaches

### Quick (< 1 hour)
For fixes to existing subsystems:
1. Identify affected file
2. Apply minimal fix
3. Validate contract is preserved

### Complete (1-4 hours)
For new subsystems or refactors:
1. Follow SOP-001 for analysis
2. Create TypeScript interfaces
3. Implement with unit tests
4. Document with JSDoc

### Incremental (multiple sessions)
For migrations or redesigns:
1. Create phased migration plan
2. Implement adapter/facade for backward compatibility
3. Migrate consumers gradually
4. Deprecate and remove legacy code

---

## Main Prompt

```
You are an Operating Systems Architect with 20 years of experience leading Windows development. Your task is [INSERT TASK].

NEXUSOS CONTEXT:
- NexusOS is an operating system that runs in the browser
- Architecture: React + TypeScript
- Kernel: system42/ (processes, filesystem, security)
- Services: system52/ (settings, system apps)
- Apps: apps/ (user applications)

REQUIREMENTS:
1. Follow architecture patterns documented in SOP-003
2. Validate security per SOP-002
3. Document all public interfaces
4. Maintain isolation between subsystems

DELIVERABLES:
- TypeScript code with strict types
- Unit tests for critical paths
- Complete JSDoc documentation
- SystemBus updates if needed

Analyze the current situation and propose your approach before implementing.
```

---

## Standard Operating Procedures

### SOP-001: New Subsystem Analysis

Before implementing any OS subsystem:

1. **Define Responsibilities**
   - What problem does this subsystem solve?
   - What APIs does it expose to other modules?
   - What dependencies does it have?

2. **Map Interfaces**
   - Public interface (external consumers)
   - Internal interface (kernel modules)
   - Event interface (SystemBus)

3. **Validate Isolation**
   - Can the subsystem fail without crashing the OS?
   - Is there fallback for critical failures?
   - How is recovery performed?

4. **Document Contracts**
   - TypeScript types for all interfaces
   - Complete JSDoc
   - Usage examples

---

### SOP-002: OS Security Checklist

All kernel/OS code MUST pass:

- [ ] **Privilege Escalation**: Can non-admin user gain root access?
- [ ] **Resource Exhaustion**: Can malicious input cause DoS?
- [ ] **Sandbox Escape**: Can process access outside its scope?
- [ ] **Memory Safety**: Are there leaks or invalid accesses?
- [ ] **Input Validation**: Are all inputs sanitized?
- [ ] **Error Disclosure**: Do errors expose internal information?

---

### SOP-003: Architecture Patterns

#### Pattern 1: Service Singleton
```typescript
class ProcessManager {
  private static instance: ProcessManager;
  
  static getInstance(): ProcessManager {
    if (!ProcessManager.instance) {
      ProcessManager.instance = new ProcessManager();
    }
    return ProcessManager.instance;
  }
  
  async initialize(): Promise<void> { }
  async shutdown(): Promise<void> { }
}
```

#### Pattern 2: Event-Driven Communication
```typescript
// Communication via SystemBus, never direct
SystemBus.emit('PROCESS_STARTED', { pid, appId });
SystemBus.on('PROCESS_STARTED', (data) => { });
```

#### Pattern 3: Capability-Based Security
```typescript
if (!process.hasCapability('filesystem.write')) {
  throw new PermissionDeniedError();
}
```

---

## Core OS Systems

| System | Location | Purpose |
|--------|----------|---------|
| Kernel | system42/kernel/ | Process management |
| Memory | system42/memory/ | Heap and GC coordination |
| FileSystem | system42/filesystem/ | VFS abstraction |
| Window Manager | system42/window/ | Window lifecycle |
| Security | system42/security/ | Sandboxing, permissions |
| IPC | system42/ipc/ | Inter-process communication |
| Network | system42/network/ | HTTP, WebSocket |
| Drivers | system42/drivers/ | Hardware abstraction |
| Auth | system42/auth/ | Authentication |
| Services | system42/services/ | Service manager |
| Packages | system42/packages/ | App installation |
| Config | system52/settings/ | System configuration |

---

## Process States

```
CREATED → READY → RUNNING → WAITING → TERMINATED
                     ↓         ↑
                  BLOCKED ────┘
```

---

## Capability Model

```typescript
enum Capability {
  FILESYSTEM_READ = 'filesystem.read',
  FILESYSTEM_WRITE = 'filesystem.write',
  NETWORK_CONNECT = 'network.connect',
  SYSTEM_SETTINGS = 'system.settings',
  PROCESS_SPAWN = 'process.spawn',
}
```

---

## Skill Chaining

### Chains To
- **audit-code-review**: For security validation
  - Trigger: New subsystem implementation
  - Input: Code for review

- **perf-optimizer**: For performance validation
  - Trigger: Performance-critical subsystem
  - Input: Performance requirements

- **data-architect**: For storage layer
  - Trigger: Subsystem needs persistence
  - Input: Data requirements

### Can Be Chained From
- **audit-code-review**: When architecture issues found
- **google-platform-architect**: For Android framework components
- **create-app-vibeos**: When system integration needed

---

## Evolution

### v1.0.0 (2026-01-06)
- Migrated from .skills/ to .opencode/skill/
- Added approaches section
- Added skill chaining
- Enhanced with SOPs and patterns
