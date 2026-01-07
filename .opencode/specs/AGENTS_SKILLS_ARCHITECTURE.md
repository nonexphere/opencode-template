# OpenCode Agents & Skills Architecture

<!-- @META: Architecture Specs -->
<!--
    File: .opencode/specs/AGENTS_SKILLS_ARCHITECTURE.md
    Version: 1.1.0
    Created: 2026-01-06
    Updated: 2026-01-07
    Scope: Architecture of agents and skills for NexusOS
-->

<!-- @NOTE(arch-def): Definition -->
> Arquitetura completa de agents e skills para NexusOS/BrasaAI

## 1. Visao Geral

<!-- @NOTE(overview): Overview -->
Este documento descreve a arquitetura de agents e skills criados para o OpenCode, destilados dos repositorios:
- `nexus-os-dash` - Monorepo principal (17 skills, 9 RFCs, 14 auditorias)
- `roocode-configs` - Configuracoes de agents (11 ativos, 33 WIP, 30+ SOPs)

---

## 2. Arquitetura de Modelos

<!-- @SCHEMA: Architecture Diagram -->
```
┌─────────────────────────────────────────────────────────────┐
│                    ORCHESTRATION LAYER                       │
│                                                              │
│    ┌──────────────────────────────────────────────────┐     │
│    │         Leonidas (Gemini 3 Pro High)             │     │
│    │         1M context - Visao Global                │     │
│    └──────────────────┬───────────────────────────────┘     │
│                       │                                      │
└───────────────────────┼──────────────────────────────────────┘
                        │
        ┌───────────────┼───────────────┐
        │               │               │
        ▼               ▼               ▼
┌───────────────┐ ┌───────────┐ ┌───────────────┐
│  PLANNING     │ │  CODING   │ │   RESEARCH    │
│ Claude Opus   │ │Claude Opus│ │ Gemini 3 Flash│
│ Thinking High │ │Think Med  │ │               │
└───────────────┘ └───────────┘ └───────────────┘
        │               │               │
        ▼               ▼               ▼
┌───────────────┐ ┌───────────┐ ┌───────────────┐
│ - planning    │ │ - code    │ │ - research    │
│ - architect   │ │ - debug   │ │               │
└───────────────┘ └───────────┘ └───────────────┘
```

---

## 3. Agents Criados

### 3.1 Leonidas (Orchestrator)
<!-- @REF(.opencode/agent/leonidas.md): Leonidas -->
| Campo | Valor |
|-------|-------|
| **Arquivo** | `.opencode/agent/leonidas.md` |
| **Modelo** | `google/antigravity-gemini-3-pro-high` |
| **Contexto** | 1,048,576 tokens |
| **Proposito** | Orquestracao autonoma de projetos |
| **SOPs** | LEO-001 a LEO-008 |

### 3.2 Architect
<!-- @REF(.opencode/agent/architect.md): Architect -->
| Campo | Valor |
|-------|-------|
| **Arquivo** | `.opencode/agent/architect.md` |
| **Modelo** | `google/antigravity-claude-opus-4-5-thinking-high` |
| **Contexto** | 200,000 tokens |
| **Proposito** | Design de sistemas (apenas .md) |
| **SOPs** | ARC-001 |

### 3.3 Code
<!-- @REF(.opencode/agent/code.md): Code -->
| Campo | Valor |
|-------|-------|
| **Arquivo** | `.opencode/agent/code.md` |
| **Modelo** | `google/antigravity-claude-opus-4-5-thinking-medium` |
| **Contexto** | 200,000 tokens |
| **Proposito** | Implementacao e testes |
| **SOPs** | CODE-001 (6 fases) |

### 3.4 Debug
<!-- @REF(.opencode/agent/debug.md): Debug -->
| Campo | Valor |
|-------|-------|
| **Arquivo** | `.opencode/agent/debug.md` |
| **Modelo** | `google/antigravity-claude-opus-4-5-thinking-high` |
| **Contexto** | 200,000 tokens |
| **Proposito** | Debugging sistematico |
| **SOPs** | DEBUG-001 (6 fases) |

### 3.5 Research
<!-- @REF(.opencode/agent/research.md): Research -->
| Campo | Valor |
|-------|-------|
| **Arquivo** | `.opencode/agent/research.md` |
| **Modelo** | `google/antigravity-gemini-3-flash` |
| **Contexto** | 1,048,576 tokens |
| **Proposito** | Investigacao de codebase |
| **SOPs** | RSH-001 a RSH-006 |

### 3.6 Planning
<!-- @REF(.opencode/agent/planning.md): Planning -->
| Campo | Valor |
|-------|-------|
| **Arquivo** | `.opencode/agent/planning.md` |
| **Modelo** | `google/antigravity-claude-opus-4-5-thinking-high` |
| **Contexto** | 200,000 tokens |
| **Proposito** | Planejamento estrategico e RFCs |
| **SOPs** | PLN-001 a PLN-006 |

---

## 4. Skills Criadas

<!-- @REF(.opencode/skill/): Skills -->
| Skill | Arquivo | Proposito |
|-------|---------|-----------|
| `code-review` | `skill/code-review/SKILL.md` | Revisao de codigo sistematica |
| `rfc-creation` | `skill/rfc-creation/SKILL.md` | Criacao de RFCs enterprise |
| `codebase-analysis` | `skill/codebase-analysis/SKILL.md` | Analise de codebases |
| `task-decomposition` | `skill/task-decomposition/SKILL.md` | Decomposicao de iniciativas |

---

## 5. Commands Criados

<!-- @REF(.opencode/command/): Commands -->
| Command | Arquivo | Modelo | Proposito |
|---------|---------|--------|-----------|
| `/analyze` | `command/analyze.md` | `google/antigravity-gemini-3-flash` | Analisar codebase |
| `/rfc` | `command/rfc.md` | `google/antigravity-claude-opus-4-5-thinking-high` | Criar RFC |
| `/decompose` | `command/decompose.md` | `google/antigravity-claude-opus-4-5-thinking-high` | Decompor tarefas |
| `/review` | `command/review.md` | `google/antigravity-claude-sonnet-4-5-thinking-medium` | Code review |

---

## 6. Estrutura Final de Diretorios

<!-- @NOTE(struct): Directory Structure -->
```
.opencode/
├── agent/
│   ├── leonidas.md        # Orchestrator (Gemini 3 Pro High)
│   ├── architect.md       # Design (Claude Opus Thinking High)
│   ├── code.md            # Implementation (Claude Opus Thinking Medium)
│   ├── debug.md           # Debugging (Claude Opus Thinking High)
│   ├── research.md        # Research (Gemini 3 Flash)
│   └── planning.md        # Planning (Claude Opus Thinking High)
│
├── skill/
│   ├── code-review/
│   │   └── SKILL.md
│   ├── rfc-creation/
│   │   └── SKILL.md
│   ├── codebase-analysis/
│   │   └── SKILL.md
│   └── task-decomposition/
│       └── SKILL.md
│
├── command/
│   ├── analyze.md         # /analyze (Gemini Flash)
│   ├── rfc.md             # /rfc (Claude Opus High)
│   ├── decompose.md       # /decompose (Claude Opus High)
│   └── review.md          # /review (Claude Sonnet Medium)
│
└── specs/
    ├── AVAILABLE_MODELS.md
    ├── ORCHESTRATION_ARCHITECTURE.md
    ├── OPENCODE_FORMAT_REFERENCE.md
    └── AGENTS_SKILLS_ARCHITECTURE.md  # Este arquivo
```

---

## 7. Padroes e Convencoes

### 7.1 Modelos por Tipo de Tarefa

<!-- @RULE: Model Selection -->
| Tarefa | Modelo | Thinking Level |
|--------|--------|----------------|
| Orquestracao | `antigravity-gemini-3-pro-high` | N/A (1M context) |
| Arquitetura | `claude-opus-4-5-thinking-high` | High |
| Planejamento | `claude-opus-4-5-thinking-high` | High |
| Debugging | `claude-opus-4-5-thinking-high` | High |
| Implementacao | `claude-opus-4-5-thinking-medium` | Medium |
| Code Review | `claude-sonnet-4-5-thinking-medium` | Medium |
| Pesquisa | `antigravity-gemini-3-flash` | N/A (1M context) |

### 7.2 Confidence Levels

<!-- @NOTE(confidence): Levels -->
- 🟢 HIGH: Multiplas fontes, verificado
- 🟡 MEDIUM: Analise suporta, nao verificado
- 🟠 LOW: Fonte unica, inferencia
- 🔴 HYPOTHESIS: Teoria a investigar

### 7.3 Severidade de Issues

<!-- @NOTE(severity): Severity -->
- 🔴 CRITICAL: Must fix
- 🟠 MAJOR: Should fix
- 🟡 MINOR: Nice to fix
- 🔵 SUGGESTION: Consider

---

## 8. Fontes Destiladas

### De roocode-configs:
- 11 agents ativos → 6 agents OpenCode
- 30+ SOPs → Integrados nos agents
- Templates RFC/ADR/Issue → Skills

### De nexus-os-dash:
- 17 skills existentes → 4 skills core
- 9 RFCs → Template de RFC
- Padroes de codigo → Integrados em agents

---

## 9. Proximos Passos

1. [ ] Adicionar mais skills especificas (data-architect, perf-optimizer)
2. [ ] Criar agent de GitHub integration
3. [ ] Implementar memory persistence
4. [ ] Adicionar hooks de validacao
5. [ ] Documentar workflows complexos

---

**Criado**: 2026-01-06
**Versao**: 1.1.0
**Autor**: OpenCode + Founder Directives
**Ultima Atualizacao**: Modelos Antigravity configurados
