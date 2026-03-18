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
| **Rodada** | 11 |
| **SHA código** | `477af2a` |
| **SHA status** | `477af2a` |
| **Data** | 2026-03-18 |
| **Modo** | EVOLUÇÃO DE PRODUTO — Limpeza Documental Executada |
| **build** | ✅ exit 0 (zero código de produção tocado) |
| **ONDA ativa** | ONDA 7 Fase 3+ pendente (Owner) |
| **Próxima ação obrigatória** | Owner decide: ONDA 7 Fase 3+ (knowledge_rules, dtc_analyze, OBD R4/R5) |

### Resumo da última rodada

**Rodada 11 — Limpeza Documental Executada (4 frentes):**

**1. DEPRECIADOS (banner ⚠️ LEGADO) — 15 arquivos:**
- `docs/AI/*` (8 arquivos)
- `docs/00_START_HERE.md`
- `docs/01_GOVERNANCE/AI_DEVELOPMENT_WORKFLOW.md`
- `docs/01_GOVERNANCE/architecture_rules.md`
- `docs/09_AUDIT/purge_progress.md`
- `docs/09_AUDIT/migration_planning/*` (3 arquivos)

**2. ATUALIZADOS (alinhar com GEMINI.md) — 2 arquivos:**
- `global_rules.md` §5 — milestones M16-M22 → ONDAs; AI_CONTEXT_SNAPSHOT → STATUS.md
- `SERVICES_AND_DATA_ACCESS.md` — exemplo `supabase.from()` → `queryService.from()`

**3. REMOVIDOS (lixo técnico) — 3 arquivos:**
- `docs/09_AUDIT/schema_dump_temp.sql`
- `docs/09_AUDIT/teste01.pdf`
- `docs/13_DIAGNOSTIC_ENGINE/diagnostico_mapa.md.resolved`

**4. MANTIDOS (referência histórica) — ~140 arquivos**

---

## ✅ HIERARQUIA DOCUMENTAL EFETIVADA (D-011)

| Camada | Escopo | Arquivos |
|--------|--------|----------|
| **1 — Governança** (LEI) | Manda em tudo | `GEMINI.md`, `STATUS.md`, `IDENTITY_CONTRACT`, `ROLE_PERMISSION_MATRIX` |
| **2 — Direção Estratégica** | Visão IA | `docs/10_AI_STRATEGY/*` |
| **3 — Referência** | Consulta | `docs/02_PRODUCT/`, `docs/03_DB/`, contratos canônicos |
| **4 — Execução** | Agentes/Operação | `.agents/rules/`, `.agents/skills/`, `.agents/workflows/` |
| **❌ Legado** (NÃO guia) | Banner ⚠️ | `docs/AI/*`, `00_START_HERE`, `architecture_rules`, `AI_DEVELOPMENT_WORKFLOW` |

---

## 8. DECISÕES FORMAIS REGISTRADAS

| ID | Decisão | Data |
|----|---------|------|
| D-001 | Catálogo MO, Peças, Cotações e Financeiro são **web-only** | 2026-03-17 |
| D-002 | queryService é o único ponto de acesso a tabelas | 2026-03-17 |
| D-003 | logger é o único ponto de log. console.* banido | 2026-03-18 |
| D-004 | Modo Evolução de Produto ativo desde 2026-03-16 | 2026-03-16 |
| D-005 | Multi-role implementado. ROLE_PERMISSION_MATRIX canônico | 2026-03-17 |
| D-006 | WhatsApp usa short OS codes | 2026-03-13 |
| D-007 | Repo público YouAutoCar-Status canal oficial ChatGPT | 2026-03-17 |
| D-008 | Hierarquia: GEMINI=lei, STATUS=memória, 10_AI_STRATEGY=direção | 2026-03-18 |
| D-009 | docs/10_AI_STRATEGY/ obrigatório antes de features IA | 2026-03-18 |
| D-010 | Autoridade documental formalizada. docs/AI/ é LEGADO | 2026-03-18 |
| D-011 | Limpeza documental executada. Hierarquia 5 camadas efetivada | 2026-03-18 |

---

## 9. RISCOS RESIDUAIS ABERTOS

| ID | Risco | Gravidade | Status |
|----|-------|:---------:|--------|
| R-001 | RPC `import_quotation_prices` sem tipo gerado | MÉDIO | EX-005 aceito |
| R-002 | 3 Dependabot alerts (1 critical, 1 high, 1 moderate) | ALTO | Owner decide |
| R-003 | Edge Function `test-pdf` sem consumidores | MÉDIO | Owner decide |
| R-004 | 2 migrations com timestamp igual | MÉDIO | Owner decide |
| R-005 a R-011 | Todos resolvidos | — | ✅ |

---

## 10. PROMPT SUGERIDO PARA PRÓXIMA SESSÃO

**Próxima frente — ONDA 7 Fase 3+ (código):**
```
Antigravity, ONDA 7 Fase 2 concluída + Limpeza Documental concluída (Rodada 11).

Decisões Owner necessárias para prosseguir:
1. CRUD `knowledge_rules` — web-only ou web+mobile?
2. Multi-tenant `knowledge_rules` — regras por company ou globais?
3. Unificação `analisar-dtc` vs `dtc_analyze` — auditar duplicação
4. OBD Center R4/R5 mobile — integração com Knowledge Base
```

### Itens Owner (paralelos):
1. **Dependabot alerts** — 1 critical, 1 high, 1 moderate
2. **Edge Function `test-pdf`** — remover?
3. **Migrations duplicadas** `20260310000000` — renomear?
4. **CRUD knowledge_rules** — decisão de escopo

---
