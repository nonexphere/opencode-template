# OpenCode Specifications Index

<!-- @META: Specifications Master Index -->
<!--
    File: .opencode/specs/README.md
    Version: 1.0.0
    Created: 2026-01-07
    Updated: 2026-01-07
    Scope: Master index for all OpenCode and NexusOS specifications
-->

> **This directory contains all architecture specifications, doctrine documents, and governance policies for the OpenCode agent system and NexusOS project.**

---

## Quick Navigation

| Category | Description | Count |
|----------|-------------|-------|
| [Root Specs](#root-specifications) | Core OpenCode system specs | 5 |
| [Architecture](#architecture) | Platform-specific architecture | 3 |
| [Doctrine](#doctrine) | Enterprise strategy and standards | 3 |
| [Governance](#governance) | RFC and spec management | 1 |
| [Diagrams](#diagrams) | Mermaid architecture diagrams | 4 |

---

## Root Specifications

<!-- @NOTE(root): Core OpenCode system specifications -->

| Spec | Description | Status |
|------|-------------|--------|
| [OPENCODE_ARCHITECTURE.md](./OPENCODE_ARCHITECTURE.md) | Complete OpenCode agent orchestration architecture | Live |
| [ORCHESTRATION_ARCHITECTURE.md](./ORCHESTRATION_ARCHITECTURE.md) | Three-tier orchestration model (Genesis → Leonidas → Specialists) | Live |
| [AGENTS_SKILLS_ARCHITECTURE.md](./AGENTS_SKILLS_ARCHITECTURE.md) | Agent and skill definitions and patterns | Live |
| [WORKFLOW_PATTERNS.md](./WORKFLOW_PATTERNS.md) | Workflow engine patterns and customization | Live |
| [AVAILABLE_MODELS.md](./AVAILABLE_MODELS.md) | AI model reference (Gemini, Claude via Antigravity) | Live |
| [OPENCODE_FORMAT_REFERENCE.md](./OPENCODE_FORMAT_REFERENCE.md) | File format specs for agents, skills, commands | Live |

---

## Architecture

<!-- @NOTE(arch): Platform-specific architecture documentation -->

Directory: `./architecture/`

| Spec | Description | Domain |
|------|-------------|--------|
| [VIBEOS_REACT_ARCHITECTURE.md](./architecture/VIBEOS_REACT_ARCHITECTURE.md) | Frontend OS architecture (System42 Kernel, System52 Services) | Frontend |
| [BACKEND_OPERATIONS.md](./architecture/BACKEND_OPERATIONS.md) | Backend operations manual (API, DB, Cache, Observability) | Backend |
| [MONOREPO_GOVERNANCE.md](./architecture/MONOREPO_GOVERNANCE.md) | Monorepo governance, ownership, and dependency management | Platform |

---

## Doctrine

<!-- @NOTE(doctrine): Enterprise strategy and operational standards -->

Directory: `./doctrine/`

| Spec | Description | Scope |
|------|-------------|-------|
| [ENTERPRISE_STRATEGY.md](./doctrine/ENTERPRISE_STRATEGY.md) | NexusOS strategic vision, culture, and roadmap | Strategic |
| [KNOWLEDGE_MANAGEMENT.md](./doctrine/KNOWLEDGE_MANAGEMENT.md) | Knowledge management system (KMS) and research methodology | Research |
| [DOCUMENTATION_STANDARDS.md](./doctrine/DOCUMENTATION_STANDARDS.md) | Documentation compliance framework and audit matrix | Standards |

---

## Governance

<!-- @NOTE(governance): RFC and specification management policies -->

Directory: `./governance/`

| Spec | Description | Type |
|------|-------------|------|
| [RFC_GOVERNANCE.md](./governance/RFC_GOVERNANCE.md) | RFC lifecycle, roles, templates, and master registry | Process |

---

## Diagrams

<!-- @NOTE(diagrams): Mermaid architecture diagrams -->

Directory: `./diagrams/`

| Diagram | Description | Format |
|---------|-------------|--------|
| [agent-hierarchy.mmd](./diagrams/agent-hierarchy.mmd) | Agent hierarchy and delegation flow | Mermaid |
| [memory-flow.mmd](./diagrams/memory-flow.mmd) | Memory and context data flow | Mermaid |
| [system-overview.mmd](./diagrams/system-overview.mmd) | Complete system overview | Mermaid |
| [workflow-engine.mmd](./diagrams/workflow-engine.mmd) | Workflow engine state machine | Mermaid |

---

## Specification Categories

<!-- @NOTE(categories): How specs are organized -->

### What Goes Where

| Category | Content Type | Examples |
|----------|-------------|----------|
| **Root (/)** | OpenCode core specs | Orchestration, agents, skills, workflows |
| **architecture/** | Platform/domain architecture | Frontend, backend, monorepo |
| **doctrine/** | Strategic and operational standards | KMS, documentation, enterprise strategy |
| **governance/** | Process and lifecycle management | RFC process, spec governance |
| **diagrams/** | Visual architecture (.mmd) | System flows, hierarchies |

### What Does NOT Belong Here

| Type | Location | Reason |
|------|----------|--------|
| Agent prompts | `.opencode/agent/` | Executable, not reference |
| Skills | `.opencode/skill/` | Loadable knowledge modules |
| Operational guides | `.opencode/docs/` | How-to, not architecture |
| Task management | `.opencode/tasks/` | Ephemeral work items |

---

## Related Resources

<!-- @REF(.opencode/BOOTSTRAP.md): Session initialization -->
<!-- @REF(.opencode/agent/): Agent definitions -->
<!-- @REF(.opencode/skill/): Skill library -->
<!-- @REF(AGENTS.md): Monorepo constitution -->

| Resource | Path | Purpose |
|----------|------|---------|
| Bootstrap | `.opencode/BOOTSTRAP.md` | Session initialization protocol |
| Agents | `.opencode/agent/*.md` | Agent definitions and prompts |
| Skills | `.opencode/skill/*/SKILL.md` | Loadable knowledge modules |
| Docs | `.opencode/docs/` | Operational guides and tutorials |
| Tasks | `.opencode/tasks/` | Work item management |

---

## Maintenance

<!-- @NOTE(maintenance): How to keep specs updated -->

### Update Protocol

1. **On Architecture Change**: Update relevant spec immediately
2. **Quarterly Review**: Audit all specs for drift
3. **Version Bumps**: Update version in frontmatter on changes
4. **Diagram Sync**: Keep diagrams aligned with text specs

### Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2026-01-07 | Initial reorganization from .opencode/docs/ |

---

<!-- @EOF -->
