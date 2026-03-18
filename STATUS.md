# YouAutoCar — MEMÓRIA OPERACIONAL DO PROJETO
<!-- ARQUIVO GERENCIADO AUTOMATICAMENTE PELO AGENTE ANTIGRAVITY -->
<!-- Este arquivo é a fonte de verdade operacional entre Antigravity, ChatGPT e Owner -->
<!-- ⚠️ Não edite manualmente sem atualizar também o repo público -->
<!-- URL PÚBLICA (ChatGPT lê aqui): -->
<!-- https://raw.githubusercontent.com/You-Telecom-Provedor-de-internet/YouAutoCar-Status/main/STATUS.md -->
<!-- Repo público: https://github.com/You-Telecom-Provedor-de-internet/YouAutoCar-Status -->

---

## 🔖 ÍNDICE RÁPIDO

1. [PROTOCOLO MULTI-AGENTE](#1-protocolo-multi-agente)
2. [ESTADO ATUAL](#2-estado-atual)
3. [MAPA DE DOMÍNIOS](#3-mapa-de-domínios)
4. [ARQUITETURA — CONTRATOS OBRIGATÓRIOS](#4-arquitetura--contratos-obrigatórios)
5. [EXCEÇÕES ARQUITETURAIS VIGENTES](#5-exceções-arquiteturais-vigentes)
6. [ONDAS — PLANO DE EXECUÇÃO](#6-ondas--plano-de-execução)
7. [HISTÓRICO DE RODADAS](#7-histórico-de-rodadas)
8. [DECISÕES FORMAIS REGISTRADAS](#8-decisões-formais-registradas)
9. [RISCOS RESIDUAIS ABERTOS](#9-riscos-residuais-abertos)
10. [PROMPT SUGERIDO PARA PRÓXIMA SESSÃO](#10-prompt-sugerido-para-próxima-sessão)

---

## 1. PROTOCOLO MULTI-AGENTE

| Agente | Papel | O que pode fazer |
|--------|-------|-----------------| 
| **Antigravity** | Execução + Commit | Lê e atualiza STATUS.md, executa código, faz push para ambos os repos |
| **ChatGPT** | Consultoria + Prompts | Lê STATUS.md via URL pública, sugere próximos prompts ao Owner |
| **Owner** | Ponte + Decisão | Repassa prompts entre agentes, decide prioridades e escopo |

**URL para leitura pelo ChatGPT (raw, sem autenticação):**
```
https://raw.githubusercontent.com/You-Telecom-Provedor-de-internet/YouAutoCar-Status/main/STATUS.md
```

---

## 2. ESTADO ATUAL

| Campo | Valor |
|-------|-------|
| **Rodada** | 8B |
| **SHA código** | `cc7ec45` |
| **SHA status** | `cc7ec45` |
| **Data** | 2026-03-18 |
| **Modo** | EVOLUÇÃO DE PRODUTO — Integração Documental + ONDA 7 |
| **build** | ✅ exit 0 (tsc --noEmit 0 erros) |
| **ONDA ativa** | ONDA 7 Fase 1 ✅ — Fase 2 pendente. Integração documental ✅ |
| **Próxima ação obrigatória** | Fase 2: regenerar tipos TS + testar dashboards |

### Resumo da última rodada

**Rodada 8B — Integração Documental:**
- Hierarquia formalizada: GEMINI.md = lei, STATUS.md = memória, 10_AI_STRATEGY = direção, .agents/ = operação
- `docs/10_AI_STRATEGY/` integrado ao GEMINI.md §16 como fonte obrigatória de contexto IA
- `AGENT_MASTER_CONTROL.md` atualizado — aponta para GEMINI.md + STATUS.md (não mais snapshots antigos)
- `COMANDOS_RAPIDOS.md` atualizado — roadmap E0-E4 obsoleto substituído por referência ao STATUS.md
- `07_GOVERNANCA_DIAGNOSTICO.md` atualizado — `confirmed_repairs` marcada como ✅ existente
- 2 novas decisões formais: D-008 (hierarquia) e D-009 (10_AI_STRATEGY obrigatório)

---

## 3. MAPA DE DOMÍNIOS

### Domínios Web (resumo)

| Domínio | Service | UI | Observação |
|---------|:---:|:---:|-----------|
| Login/Auth a Fornecedores | 🟢 | 🟢 | 29 services migrados. ONDAs 1-6A ✅ |
| Knowledge Engine | 🟢 | 🟢 | ONDA 7 F1 ✅ — 5 RPCs criadas |
| Edge Functions | 🟢 | — | 34/35 fechadas |
| RLS / Banco | 🟢 | — | 167+ migrations |
| CI/CD | 🟢 | — | ci.yml + watchdog.yml verdes |
| **IA Strategy** | 🟢 | — | 8 docs integrados na governança (GEMINI.md §16) |
| **Agents/Skills** | 🟢 | — | 9 skills + 62 workflows + 5 AGE. AGENT_MASTER_CONTROL ✅ atualizado R8B |

### Domínios Mobile

| Domínio | Status | Observação |
|---------|:---:|-----------|
| OBD ELM327 | 🟢 | Engine profissional ok |
| OBD Center (UI) | 🟡 | R4/R5 pendentes |
| Catálogo/Peças/Cotações/Financeiro | 🔴 | Web-only (decisão formal D-001) |

---

## 7. HISTÓRICO DE RODADAS

| Rodada | SHA | Data | O que foi feito | build |
|--------|-----|------|-----------------|:---:|
| 8B — Integração Doc | `cc7ec45` | 2026-03-18 | GEMINI.md §16 + STATUS + AGENT_MASTER_CONTROL + 07_GOVERNANCA + COMANDOS_RAPIDOS integrados | ✅ |
| 8 — ONDA 7F1 | `f1b6ce9` | 2026-03-18 | 5 RPCs fantasma criadas. Knowledge Engine + Cross-OS desbloqueados | ✅ |
| 7 — ONDA 6A | `aeb7c3f` | 2026-03-18 | 20 components: console.*→logger. UI 100% limpa | ✅ |
| 6 — ONDA 5 | `6d500b3` | 2026-03-18 | 39 pages: console.*→logger. Zero violações | ✅ |
| 5 — ONDA 4 | `3763e8e` | 2026-03-17 | 14 services suporte migrados | ✅ |
| 4 — ONDA 3 | `e53fc55` | 2026-03-17 | 4 services migrados + towService ✅ | ✅ |
| 3 — ONDA 2 | `7efaf5e` | 2026-03-17 | appointmentService + 5 auditados | ✅ |
| 2 — ONDA 1 | `599bff4` | 2026-03-17 | 4 services críticos (63+ calls) | ✅ |
| 1 — Barramento | `3507fc1` | 2026-03-17 | Repo público + GEMINI §17 | ✅ |

---

## 8. DECISÕES FORMAIS

| ID | Decisão | Data | SHA |
|----|---------|------|-----|
| D-001 | Catálogo MO, Peças, Cotações, Financeiro = **web-only** | 2026-03-17 | — |
| D-002 | queryService = único ponto de acesso a tabelas | 2026-03-17 | `599bff4` |
| D-003 | logger = único ponto de log | 2026-03-18 | `aeb7c3f` |
| D-004 | Modo Evolução de Produto ativo | 2026-03-16 | — |
| D-005 | Multi-role + ROLE_PERMISSION_MATRIX | 2026-03-17 | — |
| D-006 | WhatsApp short OS codes | 2026-03-13 | `2f5967f` |
| D-007 | Repo público = canal ChatGPT→Antigravity | 2026-03-17 | `3507fc1` |
| D-008 | Hierarquia: GEMINI=lei, STATUS=memória, 10_AI_STRATEGY=direção, .agents/=operação | 2026-03-18 | `cc7ec45` |
| D-009 | `docs/10_AI_STRATEGY/` = fonte obrigatória antes de features IA | 2026-03-18 | `cc7ec45` |

---

## 9. RISCOS RESIDUAIS

| ID | Risco | Gravidade | Status |
|----|-------|:---------:|--------|
| R-001 | RPC `import_quotation_prices` sem tipo | MÉDIO | EX-005 aceito |
| R-002 | 3 Dependabot alerts | ALTO | Owner decide |
| R-003 | Edge Function `test-pdf` sem consumidor | MÉDIO | Owner decide |
| R-004 | 2 migrations com timestamp duplicado | MÉDIO | Owner decide |

---

## 10. PROMPT SUGERIDO PARA PRÓXIMA SESSÃO

> Copie o bloco abaixo integralmente e envie ao Antigravity.

```
Antigravity, continuando ONDA 7 — Fase 2.

Leia o STATUS.md em:
https://raw.githubusercontent.com/You-Telecom-Provedor-de-internet/YouAutoCar-Status/main/STATUS.md

Executar:
1. Regenerar tipos TS via generate_typescript_types
2. Atualizar database.ts com os novos tipos
3. Remover casts 'as unknown as ...' de knowledgeEngineService.ts e crossOsPatternsService.ts
4. Validar tsc --noEmit
5. Testar KnowledgeEngineDashboard e CrossOsPatternsDashboard no browser
6. Atualizar STATUS.md privado + público
7. Commit + push
```

### Itens Owner (paralelos):
1. **Dependabot alerts** — 1 critical, 1 high, 1 moderate
2. **Edge Function `test-pdf`** — aprovar remoção
3. **Migrations duplicadas** `20260310000000` — renomear

---
