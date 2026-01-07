# Core OS Systems - Complete Reference

**Document ID:** `os-architect/docs/os-core-systems`  
**Version:** 1.0.0  
**Purpose:** Complete list of all subsystems that compose a modern operating system

---

## Systems Index

1. [Kernel & Process Management](#1-kernel--process-management)
2. [Memory Management](#2-memory-management)
3. [File System & VFS](#3-file-system--vfs)
4. [Window Manager & Compositor](#4-window-manager--compositor)
5. [Security & Sandboxing](#5-security--sandboxing)
6. [Inter-Process Communication (IPC)](#6-inter-process-communication-ipc)
7. [Networking Stack](#7-networking-stack)
8. [Device Drivers & HAL](#8-device-drivers--hal)
9. [User Authentication & Session](#9-user-authentication--session)
10. [Service Manager & Daemons](#10-service-manager--daemons)
11. [Package Management](#11-package-management)
12. [System Configuration](#12-system-configuration)

---

## 1. Kernel & Process Management

### Responsibilities
- Process creation and destruction
- Process scheduling
- Thread management
- Context switching
- Process prioritization

### Components in NexusOS
```
system42/
├── kernel/
│   ├── ProcessManager.ts      # Process management
│   ├── Process.ts             # Process class
│   ├── Scheduler.ts           # Scheduler (future)
│   └── ProcessState.ts        # Process states
```

### Essential APIs
```typescript
interface IProcessManager {
  spawn(appId: string, args?: string[]): Promise<Process>;
  kill(pid: number): void;
  getProcess(pid: number): Process | undefined;
  listProcesses(): Process[];
  focus(pid: number): void;
}
```

### Process States
```
CREATED → READY → RUNNING → WAITING → TERMINATED
                     ↓         ↑
                  BLOCKED ────┘
```

---

## 2. Memory Management

### Responsibilities
- Memory allocation and deallocation
- Heap management
- Garbage collection coordination
- Per-process memory limits
- Memory pressure detection

### Browser Considerations
- JavaScript has automatic GC
- Focus on avoiding memory leaks
- WeakRef for intelligent caching
- Heap size monitoring

---

## 3. File System & VFS

### Responsibilities
- Storage abstraction (VFS Layer)
- CRUD operations on files/directories
- Permissions and ACLs
- Mounting different backends
- Watch/Events for changes

### Directory Hierarchy
```
/
├── home/                      # User directory
│   └── user/
│       ├── Desktop/
│       ├── Documents/
│       ├── Downloads/
│       └── Pictures/
├── system/                    # System files
│   ├── apps/                  # Installed apps
│   └── config/                # Configurations
├── tmp/                       # Temporary files
└── var/                       # Variable data
    └── log/                   # System logs
```

---

## 4. Window Manager & Compositor

### Responsibilities
- Window management (create, move, resize)
- Z-order and focus management
- Minimize, maximize, snap
- Rendering compositor
- Multi-monitor support

### Window State
```typescript
interface WindowState {
  id: string;
  pid: number;
  title: string;
  bounds: { x, y, width, height };
  zIndex: number;
  minimized: boolean;
  maximized: boolean;
  focused: boolean;
}
```

---

## 5. Security & Sandboxing

### Responsibilities
- Application sandboxing
- Permissions model (capabilities)
- Data isolation
- Rate limiting
- Audit logging

### Capabilities Model
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

## 6. Inter-Process Communication (IPC)

### Responsibilities
- Process-to-process communication
- Publish/Subscribe (Events)
- Request/Response (RPC)
- Shared memory (SharedArrayBuffer)
- Channel-based messaging

### Communication Patterns
```typescript
// Pub/Sub
SystemBus.emit('EVENT_NAME', payload);
SystemBus.on('EVENT_NAME', handler);

// RPC
const result = await rpc.call('service.method', args);

// Direct Channel
const channel = ipc.createChannel(targetPid);
channel.send(message);
```

---

## 7. Networking Stack

### Responsibilities
- HTTP/HTTPS requests
- WebSocket connections
- Fetch API abstraction
- Request caching
- Offline support

---

## 8. Device Drivers & HAL

### Responsibilities
- Hardware Abstraction Layer
- Camera, microphone access
- Geolocation
- Sensors (accelerometer, etc.)
- Gamepad API

---

## 9. User Authentication & Session

### Responsibilities
- Login/Logout
- Session management
- Lock screen
- Multi-user support
- Password/PIN/Biometric

---

## 10. Service Manager & Daemons

### Responsibilities
- Service initialization
- Health monitoring
- Restart policies
- Dependency management
- Service lifecycle

---

## 11. Package Management

### Responsibilities
- App installation
- App updates
- App removal
- Dependency resolution
- App store integration

---

## 12. System Configuration

### Responsibilities
- Settings storage
- Default values
- User preferences
- System policies
- Configuration schema

---

## System Relationships

```
    ┌─────────────────┐
    │     Kernel      │
    └────────┬────────┘
             │
    ┌────────┴────────┐
    │                 │
┌───▼───┐       ┌─────▼─────┐
│Process│       │  Memory   │
│Manager│       │  Manager  │
└───┬───┘       └───────────┘
    │
┌───▼───┐       ┌───────────┐
│Window │       │ Security  │
│Manager│◄─────►│ Sandbox   │
└───────┘       └─────┬─────┘
                      │
              ┌───────▼───────┐
              │  FileSystem   │
              └───────────────┘
```

---

> **Tip:** To add a new subsystem, use the template in `templates/new-os-component.md`
