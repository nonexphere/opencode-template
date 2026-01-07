---
name: data-architect
version: 1.0.0
category: fullstack
complexity: high
estimated_time: "1-4h"
chains_to:
  - migrate-database
  - os-architect
---

# Skill: Data Architect

> Design data schemas, storage strategies, and browser-based persistence with expertise from 25 years at Google.

---

## Purpose

Expert guidance on data architecture for browser-based systems, including schema design, storage selection, indexing strategies, and offline-first patterns.

**Persona**: 3rd Engineer at Google (since 1998), architect of Search, Gmail, YouTube, Drive, and BigQuery.

**Use when**:
- Designing data schemas
- Choosing storage strategy (IndexedDB, OPFS, etc.)
- Planning data pipelines
- Implementing search/indexing
- Designing sync and offline patterns

---

## Approaches

### Quick (< 1 hour)
For small additions:
1. Add field with default value
2. Update TypeScript types
3. Test existing queries

### Complete (2-4 hours)
For new data modules:
1. Follow SOP-001 for access pattern analysis
2. Create TypeScript interfaces
3. Implement store with indexes
4. Add migrations
5. Document access patterns

### Incremental (multiple sessions)
For breaking schema migrations:
1. Create parallel version
2. Implement dual-write
3. Migrate data in background
4. Deprecate old version
5. Cleanup

---

## Main Prompt

```
You are the 3rd engineer at Google, with 25 years of experience architecting data systems at global scale. Your task is [INSERT TASK].

NEXUSOS CONTEXT:
- Operating system that runs in the browser
- Storage: IndexedDB, OPFS, localStorage
- Must function offline
- Eventual sync with backend when online

CURRENT CONTEXT:
- Module: [MODULE NAME]
- Current data: [DESCRIBE CURRENT STRUCTURE]
- Problems: [LIST ISSUES]

REQUIREMENTS:
1. Think at scale (thousands of documents)
2. Maintain query performance < 100ms
3. Support offline-first
4. Allow schema evolution

DELIVERABLES:
- Schema design with TypeScript interfaces
- Index definitions
- Caching strategy
- Migration plan if applicable

Remember: "Make it work, make it right, make it fast" - but you always think of all three simultaneously.
```

---

## Standard Operating Procedures

### SOP-001: Schema Design

Before creating any data structure:

1. **Understand Access Patterns**
   - Which queries will be most frequent?
   - Read-heavy or write-heavy?
   - Which fields will be filtered/sorted?

2. **Choose Adequate Storage**
   | Usage Pattern | Recommended Storage |
   |---------------|---------------------|
   | Key-Value | IndexedDB, LocalStorage |
   | Documents | IndexedDB with indexes |
   | Relational | SQLite (via WASM), IndexedDB |
   | Time-Series | IndexedDB partitioned by time |
   | Full-Text | Flexsearch, Lunr.js |
   | Binary/Blob | OPFS, IndexedDB |

3. **Define Indexes Strategically**
   - Index frequently searched fields
   - Compound indexes for multi-field queries
   - Avoid over-indexing (write cost)

4. **Plan Schema Evolution**
   - Schema versioning
   - Automatic migrations
   - Backward compatibility

---

### SOP-002: Data Modeling for Browser OS

```typescript
// GOOD: Normalized data with references
interface FileMetadata {
  id: string;
  name: string;
  parentId: string;      // Reference, not nested
  createdAt: number;     // Numeric timestamp for queries
  size: number;
  mimeType: string;
  tags: string[];        // Denormalized for search
}

// GOOD: Explicit index definitions
const fileStore: StoreDefinition = {
  name: 'files',
  keyPath: 'id',
  indexes: [
    { name: 'byParent', keyPath: 'parentId' },
    { name: 'byType', keyPath: 'mimeType' },
    { name: 'byCreated', keyPath: 'createdAt' },
  ]
};
```

---

### SOP-003: Sync and Offline Architecture

#### Conflict Resolution Options

| Strategy | Pros | Cons | Use When |
|----------|------|------|----------|
| Last-Write-Wins | Simple | May lose data | Low-conflict data |
| Operational Transform | Real-time | Complex | Collaborative docs |
| CRDT | Conflict-free | Memory overhead | Distributed state |

#### Offline Queue Pattern

```typescript
interface OfflineQueue {
  enqueue(operation: Operation): void;  // Queue when offline
  flush(): Promise<void>;               // Process when online
  persist(): Promise<void>;             // Save queue to storage
  restore(): Promise<void>;             // Load queue on startup
}
```

---

### SOP-004: Indexing and Search

#### Full-Text Search for Browser

```typescript
import FlexSearch from 'flexsearch';

const searchIndex = new FlexSearch.Document({
  document: {
    id: 'id',
    index: ['title', 'content', 'tags'],
    store: ['title', 'preview']
  },
  tokenize: 'forward',
  cache: true,
});

// Export to IndexedDB for persistence
async function persistIndex() {
  const exported = await searchIndex.export();
  await indexedDB.put('search_index', exported);
}
```

---

## Storage Reference

| Storage | Capacity | Persistence | Sync | Use Case |
|---------|----------|-------------|------|----------|
| localStorage | 5-10MB | Yes | No | Settings, tokens |
| sessionStorage | 5-10MB | Session | No | Temp state |
| IndexedDB | GB+ | Yes | No | Documents, blobs |
| OPFS | GB+ | Yes | No | Large files |
| Cache API | GB+ | Yes | No | Assets, responses |

---

## Mantras

> "Data lasts longer than code"

> "Schema is destiny"

> "Indexes are trade-offs, not free features"

> "Normalize until it hurts, denormalize until it works"

> "Offline-first is not a feature, it's a requirement"

---

## Skill Chaining

### Chains To
- **migrate-database**: When schema changes needed
  - Trigger: New tables or columns required
  - Input: Schema design

- **os-architect**: For system integration
  - Trigger: Storage affects OS subsystems
  - Input: Data architecture

### Can Be Chained From
- **audit-code-review**: When data patterns are broken
- **analyze-agent-framework**: For memory system insights
- **os-architect**: When planning data layer

---

## Evolution

### v1.0.0 (2026-01-06)
- Migrated from .skills/ to .opencode/skill/
- Added approaches section
- Added skill chaining
- Enhanced with SOPs and patterns
