---
name: analyze-agent-framework
version: 2.0.0
category: meta
complexity: high
estimated_time: "8-16h"
chains_to:
  - extract-patterns
  - rfc-creation
  - data-architect
---

# Skill: Analyze Agent Framework

> Systematic methodology for deeply analyzing AI agent frameworks, SDKs, and protocols to extract features, patterns, and design decisions for NexusOS.

---

## Purpose

This skill provides a **standardized methodology** for deeply analyzing AI agent frameworks, SDKs, and protocols with the goal of **extracting features, patterns, and design decisions** to inform the development of NexusOS's own AI protocol.

### Key Objectives

1. **Feature Distillation** - Extract reusable patterns and concepts
2. **Protocol Design** - Inform NexusOS's native AI protocol
3. **Adapter Planning** - Identify integration points for optional adapters
4. **Anti-Pattern Detection** - Learn what to avoid
5. **Innovation Opportunities** - Find gaps to innovate beyond existing solutions

> **Important:** We do NOT intend to use these libraries directly. They serve as **reference implementations** and **inspiration sources** for our own system.

**Use when**:
- Analyzing new AI frameworks for feature extraction
- Designing NexusOS AI protocol components
- Planning adapters for external frameworks
- Research and competitive analysis

---

## Analysis Priority

| Priority | Framework | Rationale |
|----------|-----------|-----------|
| **P0** | `genai-processors` | **BASE** - Will be used as foundation |
| **P1** | `pydantic-ai` | Type-safe patterns, MCP support |
| **P1** | `pydantic-graph` | Workflow/graph patterns |
| **P1** | `mcp-python-sdk` | Protocol reference |
| **P2** | `anthropic-sdk` | Tool use patterns |
| **P2** | `agno` | Multi-modal patterns |
| **P3** | `llama-index` | RAG patterns |
| **P3** | `langchain` | What to avoid + some patterns |

---

## Approaches

### Quick (1 hour)
For initial assessment of a framework:
1. Read README and documentation
2. Identify core components
3. Note unique features
4. Quick assessment of fit for NexusOS

### Complete (8-16 hours)
Full 10-phase deep analysis:
1. Initial Discovery (1h)
2. Architecture Deep Dive (2-3h)
3. Engine/Loop Analysis (2-3h)
4. Tool System Deep Dive (2-3h)
5. Memory & Context (1-2h)
6. LLM Integration (2h)
7. Protocol Analysis (1-2h)
8. Unique Mechanisms (1-2h)
9. Feature Extraction (2-3h) - CRITICAL
10. Documentation & Synthesis (2h)

### Incremental (multiple sessions)
For very large frameworks:
1. Session 1: Discovery + Architecture
2. Session 2: Engine + Tools
3. Session 3: Memory + LLM
4. Session 4: Protocols + Unique
5. Session 5: Feature Extraction + Docs

---

## Main Prompt

```
Perform a **deep analysis** of the specified AI agent framework following the 10-phase methodology.

## Target Framework
[Framework name, repository URL, version]

## Analysis Focus (Optional)
[e.g., "Engine loop", "Tool system", "MCP support"]

---

## Phase 1: Initial Discovery (1 hour)

- [ ] Read all documentation (README, docs, website)
- [ ] Identify package structure and entry points
- [ ] List all dependencies and their purposes
- [ ] Understand design philosophy and goals
- [ ] Note versioning and stability
- [ ] Identify maintainers and community

---

## Phase 2: Architecture Deep Dive (2-3 hours)

- [ ] Map all core components/classes
- [ ] Document inheritance hierarchies
- [ ] Identify interfaces and abstract classes
- [ ] Map component communication patterns
- [ ] Identify design patterns (Factory, Singleton, etc.)
- [ ] Document configuration system
- [ ] Identify extension points
- [ ] Create architecture diagrams

---

## Phase 3: Engine/Loop Analysis (2-3 hours)

- [ ] Find main execution entry point
- [ ] Trace complete execution flow
- [ ] Document each phase/stage
- [ ] Identify decision points and branching
- [ ] Analyze state management
- [ ] Document loop termination conditions
- [ ] Analyze retry and recovery logic
- [ ] Document streaming implementation
- [ ] Identify async/await patterns
- [ ] Benchmark performance if possible

---

## Phase 4: Tool System Deep Dive (2-3 hours)

- [ ] Document tool definition formats (ALL variations)
- [ ] Analyze schema generation
- [ ] Document registration mechanisms
- [ ] Trace execution flow step-by-step
- [ ] Analyze input validation (when, how, what)
- [ ] Analyze output processing
- [ ] Document error handling patterns
- [ ] Analyze async tool support
- [ ] Analyze parallel execution
- [ ] Document timeout handling
- [ ] Identify security measures
- [ ] Document sandboxing (if any)

---

## Phase 5: Memory & Context (1-2 hours)

- [ ] Identify all memory types
- [ ] Document short-term memory implementation
- [ ] Document long-term memory (if exists)
- [ ] Analyze context window management
- [ ] Document summarization strategies
- [ ] Analyze token counting
- [ ] Document persistence options
- [ ] Identify caching patterns

---

## Phase 6: LLM Integration (2 hours)

- [ ] List all supported providers
- [ ] Document abstraction layer design
- [ ] Analyze per-provider implementations
- [ ] Document streaming support per provider
- [ ] Analyze function calling formats
- [ ] Document retry/fallback logic
- [ ] Analyze rate limiting handling
- [ ] Document cost tracking (if any)
- [ ] Analyze model switching capabilities

---

## Phase 7: Protocol Analysis (1-2 hours)

- [ ] Identify implemented protocols
- [ ] Document MCP support (if any)
- [ ] Analyze OpenAI function format compatibility
- [ ] Document message formats
- [ ] Analyze serialization/deserialization
- [ ] Document transport layers
- [ ] Identify authentication patterns

---

## Phase 8: Unique Mechanisms (1-2 hours)

- [ ] Identify ALL novel features
- [ ] Document each unique mechanism in detail
- [ ] Analyze how they're implemented
- [ ] Evaluate their value for NexusOS
- [ ] Note any patents or licensing concerns

---

## Phase 9: Feature Extraction (2-3 hours) - CRITICAL

- [ ] List ALL patterns worth adopting
- [ ] Rate each pattern (must-have, nice-to-have, optional)
- [ ] Document anti-patterns to avoid
- [ ] Identify innovation opportunities
- [ ] Define adapter requirements (if applicable)
- [ ] Document implications for NexusOS protocol
- [ ] Create comparison with our target design

---

## Phase 10: Documentation & Synthesis (2 hours)

- [ ] Write comprehensive README
- [ ] Complete spec.yaml
- [ ] Create all required markdown files
- [ ] Add code snippets
- [ ] Create diagrams
- [ ] Review for completeness
- [ ] Cross-reference with other frameworks

---

## Output Structure

```
wiki/index-agents/{framework-name}/
├── README.md                        # Overview and decision summary
├── spec.yaml                        # Machine-readable specification
├── architecture/                    # Architecture docs
├── engine/                          # Engine loop analysis
├── tools/                           # Tool system docs
├── memory/                          # Memory system docs
├── providers/                       # LLM provider docs
├── protocols/                       # Protocol docs
├── unique-mechanisms/               # Unique features
├── feature-extraction/              # KEY OUTPUT
│   ├── patterns-to-adopt.md
│   ├── patterns-to-avoid.md
│   ├── innovation-gaps.md
│   └── nexus-implications.md
├── code-references/                 # Key source files
└── examples/                        # Usage examples
```

---

## Constraints

- **Be thorough**: This is research, not implementation
- **Document everything**: Future you will thank present you
- **Source code is truth**: Docs can be outdated
- **Compare objectively**: No framework is perfect
```

---

## Feature Extraction Template

### Patterns to Adopt

```yaml
patterns_to_adopt:
  - name: "Pattern Name"
    category: engine | tool | memory | provider | protocol
    priority: must-have | nice-to-have | optional
    description: "What this pattern does"
    implementation_notes: "How they implement it"
    nexus_adaptation: "How we would adapt it"
    source_files:
      - "path/to/file.py"
    complexity: low | medium | high
    dependencies: []
```

### Patterns to Avoid

```yaml
patterns_to_avoid:
  - name: "Anti-pattern Name"
    category: engine | tool | memory | provider | protocol
    severity: critical | high | medium | low
    description: "What this anti-pattern is"
    why_avoid: "Why it's problematic"
    observed_in:
      - "path/to/file.py"
    alternative: "What to do instead"
```

### Innovation Gaps

```yaml
innovation_gaps:
  - area: "Area name"
    current_state: "How existing frameworks handle this"
    limitation: "What's missing or suboptimal"
    nexus_opportunity: "How NexusOS can do better"
    complexity: low | medium | high
    impact: high | medium | low
```

---

## Related Templates

See `docs/` and `templates/` subdirectories for:
- `spec.yaml` - Machine-readable specification template
- `methodology.md` - Detailed analysis methodology
- `engine-loop.md` - Loop analysis template
- `tool-system.md` - Tool system template
- `feature-extraction.yaml` - Feature extraction template

---

## Skill Chaining

### Chains To
- **extract-patterns**: After analysis, extract reusable patterns
  - Trigger: Completed feature-extraction phase
  - Input: Patterns identified

- **rfc-creation**: When major architectural decision needed
  - Trigger: Innovation opportunity identified
  - Input: Gap analysis

- **data-architect**: For data/storage patterns
  - Trigger: Memory system insights
  - Input: Storage patterns found

### Can Be Chained From
- **codebase-analysis**: To understand before analyzing
- **rfc-creation**: When RFC requires framework research

---

## Evolution

### v2.0.0 (2026-01-06)
- Migrated from .skills/ to .opencode/skill/
- Added approaches section
- Added skill chaining
- Enhanced with templates reference

### v1.0.0 (2026-01-03)
- Initial version with 10-phase methodology
