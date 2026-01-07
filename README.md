# OpenCode Agents Template

> A complete template for configuring a hierarchical AI agent network in [OpenCode](https://github.com/opencode-ai/opencode)

---

## What is this?

This is a **ready-to-use template/bundle** that sets up a complete AI agent network for OpenCode. It includes:

- **7 Specialized Agents** - Hierarchical orchestration with role-based specialists
- **25 Skills** - Reusable knowledge modules for common tasks
- **4 Slash Commands** - Quick access to common workflows (`/analyze`, `/review`, `/decompose`, `/rfc`)
- **SOPs** - Standard Operating Procedures for consistent behavior
- **Complete Documentation** - Architecture specs and patterns

---

## Quick Start

### 1. Copy the Template

```bash
# Clone this template repository
git clone https://github.com/your-repo/opencode-agents-template.git

# Or just copy the .opencode folder to your project
cp -r opencode-agents-template/.opencode /path/to/your-project/
```

### 2. Customize for Your Project

Edit `.opencode/BOOTSTRAP.md` with your project information:

```markdown
| Attribute | Value |
|-----------|-------|
| **Project** | Your Project Name |
| **Mission** | Your project's mission statement |
| **Owner** | Project Owner |
```

### 3. Start OpenCode

```bash
# Navigate to your project
cd your-project

# Start OpenCode
opencode
```

The agents will automatically load from `.opencode/` and be ready to use.

---

## Template Structure

```
.opencode/
├── BOOTSTRAP.md               # Session entry point (READ FIRST)
│
├── agent/                     # Agent Definitions (7 agents)
│   ├── orchestrator.md        # Main thread - human interface
│   ├── leonidas.md            # Sub-orchestrator - autonomous execution
│   ├── architect.md           # System design (specs only)
│   ├── planning.md            # Strategic planning, RFCs
│   ├── code.md                # Implementation specialist
│   ├── research.md            # Investigation and discovery
│   └── debug.md               # Root cause analysis
│
├── skill/                     # Reusable Knowledge Modules (25 skills)
│   ├── opencode-specs/        # OpenCode engine documentation
│   ├── code-review/           # Code review methodology
│   ├── rfc-creation/          # RFC writing guide
│   ├── task-decomposition/    # Breaking down initiatives
│   ├── codebase-analysis/     # Analyzing code
│   └── ...                    # Many more skills
│
├── command/                   # Slash Commands
│   ├── analyze.md             # /analyze - Codebase analysis
│   ├── review.md              # /review - Code review
│   ├── decompose.md           # /decompose - Task breakdown
│   └── rfc.md                 # /rfc - Create RFC document
│
├── memory/                    # Persistent Knowledge
│   ├── project.yaml           # Project structure metadata
│   └── README.md              # Memory system docs
│
├── tasks/                     # Work Queue
│   ├── sprint.yaml            # Current sprint tasks
│   ├── queue/                 # Ready-to-execute tasks
│   └── archive/               # Completed work history
│
├── specs/                     # Reference Documentation
│   ├── ORCHESTRATION_ARCHITECTURE.md
│   ├── AGENTS_SKILLS_ARCHITECTURE.md
│   └── AVAILABLE_MODELS.md
│
└── docs/                      # Extended Documentation
    ├── architecture.md        # Full system architecture
    └── workflows.md           # Workflow patterns guide
```

---

## Agent Hierarchy

```
┌─────────────────────────────────────────────────────────────────┐
│                         HUMAN                                    │
│                     (Project Owner)                              │
└───────────────────────────┬─────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│                    ORCHESTRATOR (Genesis)                        │
│                    Main Thread - Claude Opus                     │
│  • Human interface                                               │
│  • Task decomposition                                            │
│  • Parallel delegation                                           │
└───────────────────────────┬─────────────────────────────────────┘
                            │
            PARALLEL DECOMPOSITION (Task tool)
                            │
        ┌───────────────────┼───────────────────┐
        ▼                   ▼                   ▼
┌───────────────┐   ┌───────────────┐   ┌───────────────┐
│   LEONIDAS    │   │   LEONIDAS    │   │   LEONIDAS    │
│   Instance 1  │   │   Instance 2  │   │   Instance N  │
│   Gemini Pro  │   │   Gemini Pro  │   │   Gemini Pro  │
└───────┬───────┘   └───────┬───────┘   └───────┬───────┘
        │                   │                   │
        ▼                   ▼                   ▼
┌─────────────────────────────────────────────────────────────────┐
│                    SPECIALIST AGENTS                             │
│                                                                  │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌────────┐ │
│  │ARCHITECT │ │ PLANNING │ │   CODE   │ │ RESEARCH │ │ DEBUG  │ │
│  │  Opus    │ │   Opus   │ │   Opus   │ │  Flash   │ │  Opus  │ │
│  │  Orange  │ │  Violet  │ │  Green   │ │   Blue   │ │  Red   │ │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘ └────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

---

## Agents Reference

| Agent | Role | Model | Color |
|-------|------|-------|-------|
| **Orchestrator** | Main thread, human interface, task decomposition | Claude Opus | Gold |
| **Leonidas** | Sub-orchestrator, autonomous execution with TodoList | Gemini Pro | Purple |
| **Architect** | System design, specs, ADRs (no code) | Claude Opus | Orange |
| **Planning** | Strategic planning, RFCs, roadmaps | Claude Opus | Violet |
| **Code** | Implementation, refactoring, testing | Claude Opus | Green |
| **Research** | Investigation, codebase exploration | Gemini Flash | Blue |
| **Debug** | Root cause analysis, stack traces | Claude Opus | Red |

---

## Skills Reference

### Analysis
| Skill | Description |
|-------|-------------|
| `codebase-analysis` | Systematic codebase structure analysis |
| `extract-patterns` | Extract patterns from code |
| `analyze-agent-framework` | Analyze AI agent frameworks |

### Planning
| Skill | Description |
|-------|-------------|
| `task-decomposition` | Break initiatives into tasks |
| `rfc-creation` | Create RFC documents |
| `manage-task-boundaries` | Define task scope |

### Quality
| Skill | Description |
|-------|-------------|
| `code-review` | Systematic code review |
| `audit-code-review` | Deep code audit |
| `perf-optimizer` | Performance optimization |

### Architecture
| Skill | Description |
|-------|-------------|
| `data-architect` | Database design patterns |
| `os-architect` | OS-level architecture |
| `migrate-database` | Database migration patterns |
| `create-api` | API design patterns |

### SOPs (Standard Operating Procedures)
| Skill | Description |
|-------|-------------|
| `sop-source-citation` | Mandatory source citation protocol |
| `sop-inline-audit` | Standardized audit tags (@BUG, @VULN) |
| `sop-parallel-decomposition` | Parallel task execution |
| `sop-autonomous-operation` | Autonomous agent behavior |
| `sop-task-management` | Task lifecycle management |
| `sop-issue-tracking` | Issue tracking patterns |
| `sop-code-review-structure` | Code review structure |

### Meta
| Skill | Description |
|-------|-------------|
| `opencode-specs` | OpenCode engine documentation |
| `create-skill` | How to create new skills |
| `chain-skills` | Skill chaining patterns |
| `leonidas-protocols` | Leonidas sub-orchestrator protocols |
| `research-protocols` | Research agent protocols |

---

## Slash Commands

| Command | Description | Agent |
|---------|-------------|-------|
| `/analyze` | Analyze codebase structure and architecture | Research |
| `/review` | Perform systematic code review | Code |
| `/decompose` | Break down initiative into tasks | Planning |
| `/rfc` | Create RFC document for proposal | Planning |

---

## Customization

### Adding Your Project Context

1. **Edit BOOTSTRAP.md** - Update the project identity table and description
2. **Create AGENTS.md** - Add project-specific rules at repository root
3. **Update memory/project.yaml** - Document your project structure

### Adding New Skills

Create a new skill directory with SKILL.md:

```
.opencode/skill/my-skill/
├── SKILL.md              # Skill definition (required)
├── docs/                 # Additional docs (optional)
└── templates/            # Templates (optional)
```

### Adding New Agents

Create a new agent file in `.opencode/agent/`:

```markdown
<!-- @META: Custom Agent -->
# Agent: My Agent

## Identity
You are a specialized agent for [specific task].

## Responsibilities
- [Responsibility 1]
- [Responsibility 2]

## Constraints
- [Constraint 1]
- [Constraint 2]
```

### Adding New Commands

Create a command file in `.opencode/command/`:

```markdown
---
description: My custom command
model: anthropic/claude-sonnet
---

Execute [your command behavior].
Use the `my-skill` skill for methodology.
```

---

## Key Concepts

### Three-Tier Architecture

1. **Human Layer** - Project owner communicates with Orchestrator
2. **Orchestration Layer** - Genesis + Leonidas instances coordinate work
3. **Specialist Layer** - Focused agents execute specific tasks

### Parallel Decomposition

Complex tasks are automatically decomposed into parallel Leonidas sub-agents:

```
User Request → Orchestrator analyzes → Spawns N Leonidas instances
                                       └→ Each has own TodoList
                                       └→ Full autonomy to complete
                                       └→ Report only final result
```

### Source Citation Protocol (SOP-001)

Every piece of information must cite its source:
- `<!-- @REF(file:line) -->` in Markdown
- `source:` field in YAML tasks
- `// @REF(file)` in code

### Inline Audit Tags (SOP-002)

Standardized tags for terminal discovery:
- `@BUG` - Logic errors
- `@VULN` - Security vulnerabilities
- `@SECURITY` - Security concerns
- `@TECH-DEBT` - Technical debt
- `@TODO` - Pending tasks
- `@PERF` - Performance issues

---

## Requirements

- [OpenCode](https://github.com/opencode-ai/opencode) v0.1.0+
- API keys for configured models (Claude, Gemini)

---

## Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

---

## License

MIT License - See [LICENSE](LICENSE) for details.

---

## Resources

- [OpenCode Documentation](https://github.com/opencode-ai/opencode)
- [Agent Architecture Spec](.opencode/specs/ORCHESTRATION_ARCHITECTURE.md)
- [Skills Architecture](.opencode/specs/AGENTS_SKILLS_ARCHITECTURE.md)

---

<p align="center">
  <sub>Built with OpenCode Agent Framework</sub>
</p>
