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
10. [MISSÃO MESTRA / PRÓXIMA SESSÃO](#10-missão-mestra--próxima-sessão)

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
| **Rodada** | 7 |
| **SHA código** | `aeb7c3f` |
| **SHA status** | `5ac383f` |
| **Data** | 2026-03-18 |
| **Modo** | MISSÃO MESTRA — CONCLUÍDA ✅ |
| **build** | ✅ exit 0 |
| **ONDA ativa** | ONDA 7 — Frentes de produto (Owner decide) |

### Resumo da última rodada

**ONDA 6A executada — Limpeza técnica (components + auditoria de riscos):**
- **20 components** migrados de `console.*` → `logger.error/info/warn` (29 ocorrências)
- `import { logger }` adicionado em todos os componentes
- **ZERO console.* em toda a camada UI web (pages + components)** — D-003 expandida
- **R-002 (Dependabot):** Investigado — alertas sem PR automático. Requer Owner
- **R-003 (`test-pdf`):** Investigado — zero consumidores no código. Remoção requer Owner
- **R-004 (migrations duplicadas):** Confirmadas — renomeação requer Owner

---

## 3. MAPA DE DOMÍNIOS

### 3.1 Legenda
- 🟢 Correto e estável
- 🟡 Funcional mas com dívida técnica
- 🔴 Incompleto, quebrado ou pendente

### 3.2 Domínios Web — TODOS 🟢

| Domínio | Service | UI | Observação |
|---------|:---:|:---:|-----------|
| Login / Auth | 🟢 | 🟢 | RLS + JWT Claims. supabase.auth.* legítimo |
| Ordens de Serviço | 🟢 | 🟢 | ONDA 1 ✅ + ONDA 5 ✅ |
| Clientes | 🟢 | 🟢 | ONDA 1 ✅ + ONDA 5 ✅ |
| Veículos | 🟢 | 🟢 | ONDA 1 ✅ + ONDA 5 ✅ |
| Agendamentos | 🟢 | 🟢 | ONDA 2 ✅ + ONDA 5 ✅ |
| Financeiro | 🟢 | 🟢 | ONDA 2 ✅ + ONDA 5 ✅ |
| Catálogo MO | 🟢 | 🟢 | ONDA 2 ✅ |
| Peças | 🟢 | 🟢 | ONDA 2 ✅ + ONDA 5 ✅ |
| Pedidos de Compra | 🟢 | 🟢 | ONDA 2 ✅ + ONDA 5 ✅ |
| Cotações | 🟢 | 🟢 | ONDA 2 ✅ + ONDA 5 ✅. EX-005 (RPC) |
| WhatsApp | 🟢 | 🟢 | Hardening completo. 4 drifts corrigidos |
| OBD / DTCs | 🟢 | 🟢 | Pipeline IA restaurado. Evidence + Upload ok |
| Diagnóstico | 🟢 | 🟢 | ONDA 3 ✅ + ONDA 5 ✅. EX-001, EX-009, EX-010 |
| Diagnóstico Upload | 🟢 | 🟢 | ONDA 3 ✅. EX-001, EX-009 |
| Hipóteses | 🟢 | 🟢 | ONDA 3 ✅ |
| Evidências | 🟢 | 🟢 | ONDA 3 ✅. EX-001, EX-003, EX-009 |
| Guincho/Tow | 🟢 | 🟢 | ONDA 3 ✅. queryService em todo arquivo |
| Knowledge Engine | 🟢 | 🟢 | ONDA 4 ✅. EX-011 (4 RPCs sem tipo) |
| Scanner Context | 🟢 | 🟢 | ONDA 4 ✅. Interface local |
| Notificações | 🟢 | 🟢 | ONDA 4 ✅ + ONDA 5 ✅ |
| Auditoria | 🟢 | 🟢 | ONDA 4 ✅ + ONDA 5 ✅ |
| Health Score | 🟢 | 🟢 | ONDA 4 ✅. invokeEdgeFunction |
| Reparos Confirmados | 🟢 | 🟢 | ONDA 4 ✅. EX-001 |
| Service Intents | 🟢 | 🟢 | ONDA 4 ✅ + ONDA 5 ✅ |
| Cross-OS Patterns | 🟢 | 🟢 | ONDA 4 ✅. EX-012 (1 RPC sem tipo) |
| BI / Analytics | 🟢 | 🟢 | ONDA 4 ✅ + ONDA 5 ✅ |
| YouTube DTC | 🟢 | 🟢 | ONDA 4 ✅ → invokeEdgeFunction |
| Fornecedores | 🟢 | 🟢 | ONDA 4 ✅. eslint-disable removidos |
| Sintomas | 🟢 | 🟢 | ONDA 4 ✅ + ONDA 5 ✅ |
| Correções Hodômetro | 🟢 | 🟢 | ONDA 4 ✅ + ONDA 5 ✅ |
| Diag. Analytics | 🟢 | 🟢 | ONDA 4 ✅ + ONDA 5 ✅ |
| Dashboard/Analytics | 🟢 | 🟢 | ONDA 5 ✅ |
| Configurações | 🟢 | 🟢 | ONDA 5 ✅ |
| Observabilidade (pages) | 🟢 | 🟢 | ONDA 5 ✅ — 39 pages, ~120 console.* |
| Observabilidade (components) | 🟢 | 🟢 | ONDA 6A ✅ — 20 components, 29 console.* |
| Edge Functions | 🟢 | — | 34/35 fechadas. test-pdf: remoção Owner |
| RLS / Banco | 🟢 | — | 167 migrations. ROLE_PERMISSION_MATRIX vigente |
| CI/CD | 🟢 | — | ci.yml + watchdog.yml verdes |

### 3.3 Domínios Mobile

| Domínio | Status | Observação |
|---------|:---:|-----------|
| Login / Auth | 🟢 | Riverpod + Supabase Auth ok |
| OBD ELM327 | 🟢 | Bluetooth RFCOMM. ELM327 commands ok |
| OBD Center (UI) | 🟡 | R4/R5 pendentes — OBD Knowledge Base |
| Viagens / Telemetria | 🟢 | Trip sessions ok |
| Alertas OBD | 🟢 | Engine ok. Thresholds por perfil |
| Catálogo MO | 🔴 | Web-only — D-001 |
| Peças | 🔴 | Web-only — D-001 |
| Cotações | 🔴 | Web-only — D-001 |
| Financeiro | 🔴 | Web-only — D-001 |
| WhatsApp | N/A | Apenas backend/web |

---

## 4. ARQUITETURA — CONTRATOS OBRIGATÓRIOS

### 4.1 Contrato de acesso a dados

```
PROIBIDO                    OBRIGATÓRIO
pages/supabase.from()   →   queryService.from()
components/supabase.*   →   queryService.from()
hooks/supabase.*        →   queryService.from()
```

### 4.2 Contrato de logging

```
PROIBIDO         OBRIGATÓRIO
console.log  →   logger.info
console.error →  logger.error
console.warn  →  logger.warn
console.debug →  logger.debug
```

### 4.3 Exceções ao contrato de dados (apenas em services/)

```
supabase.auth.*      → EX-001, EX-002 (identidade/token)
supabase.functions.* → EX-003 (Edge Functions)
supabase.channel()   → EX-004 (Realtime)
supabase.storage     → EX-009 (Storage)
```

---

## 5. EXCEÇÕES ARQUITETURAIS VIGENTES

| ID | Arquivo(s) | Justificativa | Escopo |
|----|-----------|---------------|--------|
| EX-001 | múltiplos services | `supabase.auth.getUser()` — resolve identidade | Apenas user.id/email |
| EX-002 | customerService, financialService | `supabase.auth.getSession()` — access_token | Apenas para Edge Functions |
| EX-003 | customerService, vehicleService, healthService, youtubeService | `invokeEdgeFunction` — Edge Functions | Apenas funções conhecidas |
| EX-004 | serviceOrderDetailService | `supabase.channel()` — Realtime | Apenas subscription |
| EX-005 | quotationService | `supabase.rpc('import_quotation_prices')` — sem tipo gerado | Apenas esta RPC |
| EX-006 | customerService, financialService | `fetch()` com Bearer token | 3 Edge Functions específicas |
| EX-007 | SupplierPortal, CustomerApprovalPortal | Portais públicos sem auth | Apenas fetch() a EF |
| EX-008 | Login.tsx | `resetSupabaseClient()` — troca de persistência | Apenas esta função |
| EX-009 | diagnosticUploadService, evidenceService, diagnosticService | `supabase.storage` | Upload/URL/remove |
| EX-010 | diagnosticService | `supabase.rpc('delete_diagnostic_entry')` — sem tipo | Apenas esta RPC |
| EX-011 | knowledgeEngineService | 4 RPCs sem tipo gerado — cast via interface local | Apenas estas 4 RPCs |
| EX-012 | crossOsPatternsService | 1 RPC sem tipo gerado — cast via interface local | Apenas esta RPC |

---

## 6. ONDAS — PLANO DE EXECUÇÃO

### ✅ ONDA 1 — CONCLUÍDA (`599bff4` | 2026-03-17)
4 services críticos: serviceOrderService, serviceOrderDetailService, customerService, vehicleService — 63+ calls

### ✅ ONDA 2 — CONCLUÍDA (`7efaf5e` | 2026-03-17)
6 services: appointmentService migrado + 5 auditados como corretos

### ✅ ONDA 3 — CONCLUÍDA (`e53fc55` | 2026-03-17)
5 services: diagnosticService, diagnosticUploadService, hypothesisService, evidenceService, towService

### ✅ ONDA 4 — CONCLUÍDA (`3763e8e` | 2026-03-17)
14 services de suporte migrados. EX-011 e EX-012 documentadas.

### ✅ ONDA 5 — CONCLUÍDA (`6d500b3` | 2026-03-18)
39 pages web: ~120 `console.*` → `logger`. Import adicionado. Zero violações arquiteturais.

### ✅ ONDA 6A — CONCLUÍDA (`aeb7c3f` | 2026-03-18)
20 components web: 29 `console.*` → `logger`. **UI 100% limpa.** D-003 expandida.

### 📋 ONDA 7 — Frentes de produto (Owner decide)

| Frente | Pré-requisito | Estimativa |
|--------|--------------|-----------| 
| OBD Knowledge Base + Profiles + Rules + Painel | Seção 16 GEMINI.md obrigatória | Grande |
| OBD Center R4/R5 (mobile) | ONDA 7 do plano de produto | Médio |

---

## 7. HISTÓRICO DE RODADAS

| Rodada | SHA | Data | Resultado | Build |
|--------|-----|------|-----------|:---:|
| 7 — ONDA 6A | `aeb7c3f` | 2026-03-18 | 20 components: 29 console.* → logger. UI 100% limpa | ✅ |
| 6 — ONDA 5 | `6d500b3` | 2026-03-18 | 39 pages: ~120 console.* → logger. Zero violações | ✅ |
| 5 — ONDA 4 | `3763e8e` | 2026-03-17 | 14 services suporte. EX-011+EX-012 | ✅ |
| 4 — ONDA 3 | `e53fc55` | 2026-03-17 | 4 services + towService confirmado | ✅ |
| 3 — STATUS | `42687ba` | 2026-03-17 | Barramento + GEMINI.md Seção 18 | ✅ |
| 3 — ONDA 2 | `7efaf5e` | 2026-03-17 | appointmentService + 5 auditados | ✅ |
| 2 — ONDA 1 | `599bff4` | 2026-03-17 | 4 services críticos (63+ calls) | ✅ |
| 1 — Barramento | `3507fc1` | 2026-03-17 | Repo público + Seção 17 GEMINI.md | ✅ |
| — | `dd67fd0` | 2026-03-17 | Hardening A: 5 services | ✅ |
| — | `2f5967f` | 2026-03-17 | WhatsApp Inbox: 4 drifts corrigidos | ✅ |

---

## 8. DECISÕES FORMAIS REGISTRADAS

| ID | Decisão | Data | SHA |
|----|---------|------|-----|
| D-001 | Catálogo MO, Peças, Cotações e Financeiro são **web-only** | 2026-03-17 | — |
| D-002 | queryService é único ponto de acesso a tabelas. supabase direto banido em UI | 2026-03-17 | `599bff4` |
| D-003 | logger é único ponto de log. console.* banido em **services, pages e components** | 2026-03-18 | `aeb7c3f` |
| D-004 | Modo Evolução de Produto ativo desde 2026-03-16 | 2026-03-16 | — |
| D-005 | Multi-role implementado. ROLE_PERMISSION_MATRIX canônico | 2026-03-17 | — |
| D-006 | WhatsApp usa short OS codes (não heurísticas). Hardening completo | 2026-03-13 | `2f5967f` |
| D-007 | Repo público YouAutoCar-Status é canal oficial ChatGPT→Antigravity | 2026-03-17 | `3507fc1` |

---

## 9. RISCOS RESIDUAIS ABERTOS

| ID | Risco | Gravidade | Status | Ação |
|----|-------|:---------:|--------|:----:|
| R-001 | `supabase.rpc('import_quotation_prices')` sem tipo gerado | MÉDIO | EX-005 aceito | Após regen de tipos |
| R-002 | 3 Dependabot alerts (1 critical, 1 high, 1 moderate) | ALTO | Investigado — sem PR. Owner decide | Owner |
| R-003 | Edge Function `test-pdf` em produção sem consumidores | MÉDIO | Investigado — zero refs. Remoção Owner | Owner |
| R-004 | 2 migrations com mesmo timestamp `20260310000000` | MÉDIO | Confirmado — renomeação Owner | Owner |
| R-005 | ~~console.* em pages web~~ | ~~BAIXO~~ | ✅ RESOLVIDO — ONDA 5 | — |
| R-006 | ~~supabase.storage direto em services~~ | ~~BAIXO~~ | ✅ RESOLVIDO — EX-009 (ONDA 3) | — |
| R-007 | OBD Knowledge Base sem auditoria prévia (Seção 16) | MÉDIO | Bloqueado — próxima grande frente | ONDA 7+ |
| R-008 | ~~console.* em components web~~ | ~~BAIXO~~ | ✅ RESOLVIDO — ONDA 6A | — |

---

## 10. MISSÃO MESTRA / PRÓXIMA SESSÃO

## 🏆 MISSÃO MESTRA CONCLUÍDA

A camada web está **100% limpa de dívida técnica estrutural**.

| Domínio | Status Final |
|---------|:---:|
| Services (29 migrados) | 🟢 |
| Pages (39 migradas) | 🟢 |
| Components (20 migrados) | 🟢 |
| RLS / Banco | 🟢 |
| CI / Watchdog | 🟢 |

### Ações pendentes (requerem Owner)

1. **Dependabot** — 1 critical, 1 high, 1 moderate: decidir update ou suprimir
2. **`test-pdf`** — aprovar remoção: `supabase functions delete test-pdf`
3. **Migrations duplicadas** — renomear `20260310000000_fix_legacy_br_constraints.sql`

### Prompt para ONDA 7 — OBD Knowledge Base

```
Antigravity, iniciando ONDA 7 — OBD Knowledge Base.

Leia o STATUS.md em:
https://raw.githubusercontent.com/You-Telecom-Provedor-de-internet/YouAutoCar-Status/main/STATUS.md

Pré-requisito OBRIGATÓRIO: executar BLOCO 1 completo da Seção 16 do GEMINI.md
(Auditoria Pré-Implementação) antes de qualquer linha de código.

Escopos do BLOCO 1:
- Tabelas: knowledge_base, dtc_*, sensor_profiles, engine_profiles, ecu_profiles
- Services: knowledgeEngineService, crossOsPatternsService, scannerContextRecommendationService
- Edge functions: analisar-dtc, dtc_analyze, extract_document_knowledge
- Mobile: provedor de knowledge, tela OBD Center
- Perfis: EngineProfiles, EcuProfiles, SensorProfiles, ProtocolProfiles, AlertRuleProfiles

NÃO escrever código antes do BLOCO 1 entregue e validado.
```
