# Orchestration Architecture Specs

<!-- @META: Orchestration Specs -->
<!--
    File: .opencode/specs/ORCHESTRATION_ARCHITECTURE.md
    Version: 2.0.0
    Created: 2026-01-06
    Updated: 2026-01-07
    Scope: Orchestration guidelines and model architecture
-->

<!-- @NOTE(orch-def): Definition -->
> Documento de especificações para arquitetura de orquestração de agentes OpenCode

## 1. Contexto e Propósito

<!-- @NOTE(context): Context -->
Este documento destila as diretrizes do Founder para funcionamento do sistema de agentes/skills do OpenCode, aplicado ao monorepo NexusOS.

**Objetivo Principal**: Destilar doutrinas, SOPs e guidelines dos repositórios `nexus-os-dash` e `roocode-configs` em skills e agents nativos do OpenCode.

---

## 2. Arquitetura de Três Camadas

<!-- @REF(.opencode/agent/orchestrator.md): Main Thread Definition -->
<!-- @REF(.opencode/BOOTSTRAP.md#agent-hierarchy): Full Hierarchy Diagram -->

### 2.1 Visão Geral da Hierarquia

```
┌─────────────────────────────────────────────────────────────────────┐
│                         CAMADA 1: INTERFACE HUMANA                   │
│                                                                       │
│  Founder (Guilherme) ←→ Genesis Orchestrator (Main Thread)           │
│  - Recebe direções do humano                                          │
│  - Decompõe em workstreams paralelos                                  │
│  - Consolida resultados                                               │
│  - Reporta ao humano                                                  │
└───────────────────────────────────┬─────────────────────────────────┘
                                    │
            DECOMPOSIÇÃO PARALELA (SOP-003)
                                    │
┌───────────────────────────────────┴─────────────────────────────────┐
│                      CAMADA 2: SUB-ORQUESTRAÇÃO                       │
│                                                                       │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐                    │
│  │  LEONIDAS   │  │  LEONIDAS   │  │  LEONIDAS   │                    │
│  │  Instance 1 │  │  Instance 2 │  │  Instance N │                    │
│  │             │  │             │  │             │                    │
│  │ - TodoList  │  │ - TodoList  │  │ - TodoList  │                    │
│  │ - Autonomia │  │ - Autonomia │  │ - Autonomia │                    │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘                    │
│         │                │                │                           │
└─────────┼────────────────┼────────────────┼───────────────────────────┘
          │                │                │
┌─────────┴────────────────┴────────────────┴───────────────────────────┐
│                       CAMADA 3: ESPECIALISTAS                          │
│                                                                        │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐     │
│  │ Architect│ │ Planning │ │   Code   │ │ Research │ │  Debug   │     │
│  │  Opus    │ │   Opus   │ │   Opus   │ │  Flash   │ │   Opus   │     │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘ └──────────┘     │
│                                                                        │
│  Executam tarefas específicas, retornam resultados para Leonidas       │
└────────────────────────────────────────────────────────────────────────┘
```

### 2.2 Distribuição de Modelos

<!-- @SCHEMA: Model Distribution -->
| Função | Agente | Modelo | Justificativa |
|--------|--------|--------|---------------|
| **Main Thread** | Genesis Orchestrator | Claude Opus | Interface humana, decomposição estratégica |
| **Sub-Orquestrador** | Leonidas | Gemini Pro | 1M tokens de contexto, execução autônoma |
| **Planejamento** | Planning | Claude Opus | Raciocínio profundo, decisões estratégicas |
| **Arquitetura** | Architect | Claude Opus | Design de sistema, specs only (no code) |
| **Programação** | Code | Claude Opus | Qualidade de código, implementação |
| **Pesquisa** | Research | Gemini Flash | Velocidade, exploração rápida |
| **Debugging** | Debug | Claude Opus | Análise de root cause |

### 2.3 Responsabilidades por Camada

<!-- @SCHEMA: Layer Responsibilities -->
| Camada | Responsabilidade Principal | NÃO Deve Fazer |
|--------|---------------------------|----------------|
| **Genesis** | Conversar com humano, decompor, consolidar | Executar tarefas complexas |
| **Leonidas** | Orquestrar especialistas, gerenciar TodoList | Conversar com humano |
| **Specialists** | Executar tarefas atômicas | Orquestrar outros agentes |

---

## 3. Princípios de Orquestração

<!-- @RULE: Orchestration Principles -->
1. **Decomposição Paralela (SOP-003)**: Requests complexos DEVEM ser decompostos em múltiplos Leonidas paralelos
2. **Delegação Hierárquica**: Genesis → Leonidas → Specialists (nunca pular níveis)
3. **Contexto Completo**: Cada agente delegado recebe contexto completo (não tem memória)
4. **Autonomia Total**: Sub-agentes não perguntam, executam
5. **Report Only Final**: Apenas resultados finais sobem para o nível acima
6. **Paralelização Máxima**: Tasks independentes DEVEM rodar em paralelo

---

## 4. Padrões de Delegação

### 4.1 Genesis → Leonidas

<!-- @SCHEMA: Leonidas Mission Template -->
```markdown
## LEONIDAS MISSION: [Título]

### CONTEXT
[Contexto completo - Leonidas não tem memória da conversa]

### OBJECTIVE
[Deliverable único e claro]

### TASKS
1. [Task específica]
2. [Task específica]

### SUCCESS CRITERIA
- [ ] [Critério verificável]

### AUTONOMY GRANT
Você tem AUTONOMIA TOTAL:
- Crie seu próprio TodoList
- Delegue para especialistas
- Reporte APENAS o resumo final
```

### 4.2 Leonidas → Specialists

<!-- @SCHEMA: Specialist Delegation -->
```markdown
## TASK: [Título]

### CONTEXT
[Contexto específico para a tarefa]

### DELIVERABLE
[O que deve ser entregue]

### CONSTRAINTS
[Limitações e padrões a seguir]
```

---

## 5. Fontes de Doutrina

<!-- @NOTE(sources): Sources -->
### 5.1 nexus-os-dash
- `.opencode/agent/` - Definições de agentes
- `.opencode/specs/` - Especificações técnicas
- `AGENTS.md` - Constituição do monorepo
- `wiki/` - Base de conhecimento

### 5.2 Documentos Chave
<!-- @REF(AGENTS.md#sop-001): Source Citation Protocol -->
<!-- @REF(AGENTS.md#sop-002): Inline Audit Protocol -->
<!-- @REF(AGENTS.md#sop-003): Parallel Decomposition Protocol -->
<!-- @REF(.opencode/agent/orchestrator.md): Main Thread Definition -->
<!-- @REF(.opencode/BOOTSTRAP.md): Session Initialization -->

---

## 6. Changelog

| Data | Mudança | Autor |
|------|---------|-------|
| 2026-01-07 | **v2.0.0**: Arquitetura 3 camadas com Genesis Orchestrator | OpenCode |
| 2026-01-06 | v1.1.0: Adicionado modelo por função (Opus/Flash) | Founder Directive |
| 2026-01-06 | v1.0.0: Documento inicial | OpenCode |
