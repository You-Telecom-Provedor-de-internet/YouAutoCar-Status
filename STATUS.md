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

**Regras de protocolo:**
- Antigravity SEMPRE atualiza os dois STATUS.md ao final de cada rodada
- ChatGPT NUNCA sugere implementações fora do plano documentado
- Owner é o único que pode autorizar desvios de escopo
- Qualquer fato arquitetural novo descoberto durante a rodada vai direto ao STATUS.md

**URL para leitura pelo ChatGPT (raw, sem autenticação):**
```
https://raw.githubusercontent.com/You-Telecom-Provedor-de-internet/YouAutoCar-Status/main/STATUS.md
```

---

## 🔁 REGRA DE USO DO BARRAMENTO

> Esta seção é parte obrigatória da governança. Leia antes de agir.

**Este arquivo É a memória operacional do projeto.**

O sistema funciona assim:
```
GEMINI.md     = lei permanente do projeto
STATUS.md     = memória viva da sessão atual
ChatGPT       = coordena usando a memória viva
Antigravity   = executa e atualiza a memória viva
Owner         = decide prioridades e fecha o ciclo
```

**Protocolo obrigatório de abertura de sessão:**

Quando o Owner abrir uma nova sessão com o ChatGPT:
```
1. Enviar ao ChatGPT: "Sincronize pelo STATUS em
   https://raw.githubusercontent.com/You-Telecom-Provedor-de-internet/YouAutoCar-Status/main/STATUS.md
   e sugira o próximo prompt."
2. ChatGPT lê → identifica ONDA ativa + PROMPT SUGERIDO
3. Owner envia o prompt gerado ao Antigravity
4. Antigravity executa → atualiza STATUS privado + público → entrega SHA
5. Ciclo fecha
```

**O que o ChatGPT DEVE fazer ao ler este arquivo:**
- Identificar a seção ESTADO ATUAL e a ONDA ativa
- Usar o bloco PROMPT SUGERIDO como base — não inventar escopo
- Alertar o Owner se detectar divergência entre STATUS e o que o Owner descreve
- Nunca sugerir ONDAs futuras se a ONDA ativa ainda estiver aberta

**O que o Antigravity DEVE fazer ao encerrar cada rodada:**
- Atualizar `docs/audit/STATUS.md` no repo privado YouAutoCarvAPP2
- Atualizar `STATUS.md` no repo público YouAutoCar-Status
- Registrar SHA do commit, domínios afetados, riscos e prompt sugerido
- Rodada sem dupla atualização = rodada **INVÁLIDA** (GEMINI.md Seção 18)

---

## 2. ESTADO ATUAL

| Campo | Valor |
|-------|-------|
| **Rodada** | 14 |
| **SHA código** | `23ad8c3` |
| **SHA status** | `79bc4cd` |
| **Data** | 2026-03-18 |
| **Modo** | EVOLUÇÃO DE PRODUTO — ONDA 7 Fase 4 Concluída |
| **tsc** | ✅ 0 erros |
| **build** | ✅ exit 0 |
| **flutter analyze** | ✅ 0 erros |
| **ONDA ativa** | ONDA 7 Fase 5+ pendente (Owner) |
| **Próxima ação obrigatória** | Owner decide: CRUD admin web / OBD Center R4/R5 mobile |

### Resumo da última rodada

**Rodada 14 — ONDA 7 Fase 4: knowledge_rules Amadurecido (D-013):**

- ✅ Migration `20260318100000_knowledge_rules_updated_at.sql` aplicada no Supabase
- ✅ Coluna `updated_at` + trigger `set_updated_at()` adicionados
- ✅ Web: `eslint-disable as any` removido de `KnowledgeInsightsPanel.tsx` — tipagem nativa
- ✅ D-013: `knowledge_rules` é catálogo global — sem `company_id`

**Arquivos alterados:**
- `KnowledgeInsightsPanel.tsx` — removido `eslint-disable` e `as any` cast
- `20260318100000_knowledge_rules_updated_at.sql` — nova migration

---

## 3. MAPA DE DOMÍNIOS

### 3.1 Legenda
- 🟢 Correto e estável
- 🟡 Funcional mas com dívida técnica
- 🔴 Incompleto, quebrado ou pendente

### 3.2 Domínios Web

| Domínio | Service Layer | UI/Pages | Observação |
|---------|:---:|:---:|-----------|
| Login / Auth | 🟢 | 🟢 | RLS + JWT Claims corretos. supabase.auth.* é uso legítimo |
| Ordens de Serviço | 🟢 | 🟢 | ONDA 1 ✅ — serviceOrderService + serviceOrderDetailService migrados |
| Clientes | 🟢 | 🟢 | ONDA 1 ✅ — customerService migrado |
| Veículos | 🟢 | 🟢 | ONDA 1 ✅ — vehicleService migrado |
| Agendamentos | 🟢 | 🟢 | ONDA 2 ✅ + ONDA 5 ✅ — service migrado. console.* removido |
| Financeiro | 🟢 | 🟢 | ONDA 2 ✅ + ONDA 5 ✅ — service correto. console.* removido |
| Catálogo MO | 🟢 | 🟢 | ONDA 2 ✅ — laborCatalogService já correto |
| Peças | 🟢 | 🟢 | ONDA 2 ✅ + ONDA 5 ✅ — service correto. console.* removido |
| Pedidos de Compra | 🟢 | 🟢 | ONDA 2 ✅ + ONDA 5 ✅ — service correto. console.* removido |
| Cotações | 🟢 | 🟢 | ONDA 2 ✅ + ONDA 5 ✅ — service correto. EX-005 (RPC). console.* removido |
| WhatsApp | 🟢 | 🟢 | Hardening completo. 4 drifts de campo corrigidos |
| OBD / DTCs | 🟢 | 🟢 | Pipeline de IA restaurado. Evidence + Upload FASE 0-4 ok |
| Diagnóstico (service) | 🟢 | 🟢 | ONDA 3 ✅ + ONDA 5 ✅ — migrado. EX-001, EX-009, EX-010. console.* removido |
| Diagnóstico Upload (service) | 🟢 | 🟢 | ONDA 3 ✅ — migrado. EX-001, EX-009 (scan-reports bucket) |
| Hipóteses (service) | 🟢 | 🟢 | ONDA 3 ✅ — migrado. import supabase removido |
| Evidências (service) | 🟢 | 🟢 | ONDA 3 ✅ — migrado. EX-001, EX-003, EX-009 |
| Guincho/Tow | 🟢 | 🟢 | ONDA 3 ✅ — já correto. queryService em todo o arquivo |
| Knowledge Engine | 🟢 | 🟢 | ONDA 7 ✅ — migrado. ~~EX-011~~ resolvido Fase 2 |
| Scanner Context | 🟢 | 🟢 | ONDA 4 ✅ — migrado. 3 tabelas sem tipo → interface local |
| Notificações | 🟢 | 🟢 | ONDA 4 ✅ + ONDA 5 ✅ — service + pages migrados |
| Auditoria | 🟢 | 🟢 | ONDA 4 ✅ + ONDA 5 ✅ — service + pages migrados |
| Health Score | 🟢 | 🟢 | ONDA 4 ✅ — migrado. invokeEdgeFunction preservado |
| Reparos Confirmados | 🟢 | 🟢 | ONDA 4 ✅ — migrado. EX-001 preservada |
| Service Intents | 🟢 | 🟢 | ONDA 4 ✅ + ONDA 5 ✅ — service + pages migrados |
| Cross-OS Patterns | 🟢 | 🟢 | ONDA 7 ✅ — migrado. ~~EX-012~~ resolvido Fase 2 |
| BI / Analytics | 🟢 | 🟢 | ONDA 4 ✅ + ONDA 5 ✅ — service + pages migrados |
| YouTube DTC | 🟢 | 🟢 | ONDA 4 ✅ — supabase.functions.invoke→invokeEdgeFunction |
| Fornecedores | 🟢 | 🟢 | ONDA 4 ✅ — eslint-disable removidos |
| Sintomas | 🟢 | 🟢 | ONDA 4 ✅ + ONDA 5 ✅ — 8 calls + console.* removido |
| Correções Hodômetro | 🟢 | 🟢 | ONDA 4 ✅ + ONDA 5 ✅ — 4 calls + 2 RPCs + console.* removido |
| Diag. Analytics | 🟢 | 🟢 | ONDA 4 ✅ + ONDA 5 ✅ — supabase.rpc→queryService.rpc + console.* removido |
| Dashboard/Analytics | 🟢 | 🟢 | ONDA 5 ✅ — console.* → logger. import adicionado |
| Configurações | 🟢 | 🟢 | ONDA 5 ✅ — 5 console.error → logger.error |
| Observabilidade (pages) | 🟢 | 🟢 | ONDA 5 ✅ — 39 pages. ~120 console.* removidos. Zero violações |
| Observabilidade (components) | 🟢 | 🟢 | ONDA 6A ✅ — 20 components. 29 console.* removidos. UI 100% limpa |
| Edge Functions | 🟢 | — | 34/35 fechadas. test-pdf em produção (remoção: Owner) |
| RLS / Banco | 🟢 | — | 167+ migrations. Multi-role implementado. ROLE_PERMISSION_MATRIX vigente |
| CI/CD | 🟢 | — | ci.yml + watchdog.yml verdes |
| **IA Strategy** (`docs/10_AI_STRATEGY/`) | 🟢 | — | 8 docs integrados na governança (GEMINI.md §16). `07_GOVERNANCA` atualizado. |
| **Agents/Skills** (`.agents/`) | 🟢 | — | 9 skills + 62 workflows + 5 AGE ok. AGENT_MASTER_CONTROL e COMANDOS_RAPIDOS ✅ atualizados R8B |

### 3.3 Domínios Mobile

| Domínio | Status | Observação |
|---------|:---:|-----------|
| Login / Auth | 🟢 | Riverpod + Supabase Auth ok |
| OBD ELM327 | 🟢 | Bluetooth RFCOMM. ELM327 commands ok |
| OBD Center (UI) | 🟡 | R4/R5 pendentes — OBD Knowledge Base |
| Viagens / Telemetria | 🟢 | Trip sessions ok |
| Alertas OBD | 🟢 | Engine ok. Thresholds por perfil |
| Catálogo MO | 🔴 | Web-only — decisão formal |
| Peças | 🔴 | Web-only — decisão formal |
| Cotações | 🔴 | Web-only — decisão formal |
| Financeiro | 🔴 | Web-only — decisão formal |
| WhatsApp | N/A | Apenas backend/web |

---

## 4. ARQUITETURA — CONTRATOS OBRIGATÓRIOS

> Vigentes desde GEMINI.md. Nenhum código pode violar sem documentação na Seção 6.

### 4.1 Contrato de acesso a dados

```
CAMADA PROIBIDA          CAMADA OBRIGATÓRIA
pages/          ──✗──►  supabase.from()
components/     ──✗──►  supabase.from()
hooks/          ──✗──►  supabase.from()

services/       ──✓──►  queryService.from()   [para queries de tabelas]
services/       ──✓──►  supabase.auth.*        [para identidade — EXCEÇÃO]
services/       ──✓──►  supabase.functions.*   [para Edge Functions — EXCEÇÃO]
services/       ──✓──►  supabase.channel()     [para Realtime — EXCEÇÃO]
services/       ──✓──►  supabase.storage       [para Storage — EXCEÇÃO]
```

### 4.2 Contrato de logging

```
PROIBIDO         OBRIGATÓRIO
console.log  →   logger.info
console.error →  logger.error
console.warn  →  logger.warn
console.debug →  logger.debug   (ou remover)
```

### 4.3 Contrato de identidade

```
PROIBIDO                                   OBRIGATÓRIO
auth.uid() direto em RLS sem helper   →   get_user_id() helper
owner_id hardcoded               →   resolvido via RLS ou get_company_id()
```

### 4.4 Contrato de roles

```
PROIBIDO                      OBRIGATÓRIO
role === 'admin' inline   →   hasPermission() ou ROLE_PERMISSION_MATRIX
```

### 4.5 Padrão queryService — resumo técnico

```typescript
// queryService é um wrapper tipado sobre o cliente Supabase
// Exemplo de uso correto:
import { queryService } from '@/services/queryService'

const { data, error } = await queryService
  .from('tabela')
  .select('*')
  .eq('id', id)
  .single()
```

---

## 5. EXCEÇÕES ARQUITETURAIS VIGENTES

> Estas são as únicas exceções aceitas ao Contrato 4.1.
> Qualquer nova exceção deve ser adicionada aqui E na Seção 6 do GEMINI.md.

| ID | Exceção | Arquivo(s) | Justificativa | Escopo autorizado |
|----|---------|-----------|---------------|-------------------|
| EX-001 | `supabase.auth.getUser()` | múltiplos services | Resolve identidade do usuário da sessão atual. Não é query de dados. | Apenas para obter user.id ou user.email. Nunca para query de tabela. |
| EX-002 | `supabase.auth.getSession()` | customerService, financialService | Obtém access_token para autorizar chamada a Edge Function. | Apenas para extrair token. |
| EX-003 | `supabase.functions.invoke()` / `invokeEdgeFunction` | customerService, serviceOrderDetailService, vehicleService, healthService, youtubeService | Invoca Edge Functions do backend. Padrão canônico: `invokeEdgeFunction`. | Apenas para Edge Functions conhecidas e documentadas. |
| EX-004 | `supabase.channel()` / `removeChannel()` | serviceOrderDetailService | Realtime nativo. queryService não cobre Realtime. | Apenas para subscription de tabelas em tempo real. |
| EX-005 | `supabase.rpc('import_quotation_prices')` | quotationService | RPC transacional sem tipo gerado. A RPC foi adicionada via migration e ainda não está nos tipos Supabase. | Apenas esta RPC específica. Remover quando tipos forem regenerados. |
| EX-006 | `fetch()` com token | customerService, financialService | Edge Functions públicas que exigem autenticação explícita via Bearer token. | Apenas para `create_customer_crm`, `admin_update_password`, `create_recurring_transactions`. |
| EX-007 | `supabase` em portais públicos | SupplierPortal, CustomerApprovalPortal | Portais sem sessão Supabase Auth — usuário acessa via token de OS. | Apenas `fetch()` a Edge Functions públicas. Sem `.from()`. |
| EX-008 | `resetSupabaseClient()` | Login.tsx | Troca dinâmica de strategy de persistência auth (localStorage ↔ sessionStorage). | Apenas esta função. Sem query direta. |
| EX-009 | `supabase.storage` | diagnosticUploadService (scan-reports), evidenceService (diagnostic-evidence), diagnosticService (scan-reports remove) | Storage nativo — queryService não cobre Storage. | Apenas operações: upload, createSignedUrl, remove. Sem query diretas. |
| EX-010 | `supabase.rpc('delete_diagnostic_entry')` | diagnosticService | RPC transacional sem tipo gerado. By-pass de constraints fortes ao deletar entrada diagnóstica. | Apenas esta RPC. Remover quando tipos forem regenerados. |
| ~~EX-011~~ | ~~`queryService.rpc as unknown as (fn, p) => ...`~~ | ~~knowledgeEngineService~~ | ~~4 RPCs sem tipo gerado~~ | ✅ RESOLVIDO — Casts removidos na Rodada 9 (SHA `5e61f02`). queryService.rpc() genérico torna cast desnecessário. |
| ~~EX-012~~ | ~~`queryService.rpc as unknown as (fn, p) => ...`~~ | ~~crossOsPatternsService~~ | ~~1 RPC sem tipo gerado~~ | ✅ RESOLVIDO — Cast removido na Rodada 9 (SHA `5e61f02`). Same rationale as EX-011. |

---

## 6. ONDAS — PLANO DE EXECUÇÃO

### ✅ ONDA 1 — CONCLUÍDA

**SHA:** `599bff4` | **Data:** 2026-03-17 | **tsc:** 0 erros

| # | Arquivo | Calls migradas | Exceções documentadas |
|---|---------|:--------------:|----------------------|
| 1 | `serviceOrderService.ts` | 20+ | Nenhuma |
| 2 | `serviceOrderDetailService.ts` | 30+ | EX-001, EX-003, EX-004 |
| 3 | `customerService.ts` | 12+ | EX-002, EX-001, EX-003, EX-006 |
| 4 | `vehicleService.ts` | 11+ | EX-003 (`plate_lookup`) |

---

### ✅ ONDA 2 — CONCLUÍDA

**SHA:** `7efaf5e` | **Data:** 2026-03-17 | **tsc:** 0 erros

| # | Arquivo | Resultado | Detalhe |
|---|---------|:---------:|---------|
| 5 | `appointmentService.ts` | ✅ MIGRADO | 7 calls migradas. |
| 6 | `financialService.ts` | ✅ JÁ CORRETO | EX-002, EX-006 |
| 7 | `laborCatalogService.ts` | ✅ JÁ CORRETO | |
| 8 | `partService.ts` | ✅ JÁ CORRETO | |
| 9 | `purchaseOrderService.ts` | ✅ JÁ CORRETO | |
| 10 | `quotationService.ts` | ✅ JÁ CORRETO | EX-005 (RPC sem tipo) |

---

### ✅ ONDA 3 — CONCLUÍDA

**SHA:** `e53fc55` | **Data:** 2026-03-17 | **tsc:** 0 erros

| # | Arquivo | Resultado | Detalhe |
|---|---------|:---------:|---------|
| 11 | `diagnosticService.ts` | ✅ MIGRADO | 15+ calls. EX-001, EX-009, EX-010 |
| 12 | `diagnosticUploadService.ts` | ✅ MIGRADO | 8 calls. EX-001, EX-009 (scan-reports) |
| 13 | `hypothesisService.ts` | ✅ MIGRADO | 6 calls. Import supabase removido |
| 14 | `evidenceService.ts` | ✅ MIGRADO | 5 calls. EX-001, EX-003, EX-009 (3 ops) |
| 15 | `towService.ts` | ✅ JÁ CORRETO | queryService em todo o arquivo |

---

### ✅ ONDA 4 — CONCLUÍDA

**SHA:** `3763e8e` | **Data:** 2026-03-17 | **build:** exit 0

| # | Arquivo | Resultado | Detalhe |
|---|---------|:---------:|---------|
| 16 | `knowledgeEngineService.ts` | ✅ MIGRADO | 4 RPCs sem tipo → queryService.rpc+cast. EX-011 |
| 17 | `scannerContextRecommendationService.ts` | ✅ MIGRADO | 3 calls. eslint-disable → interface local |
| 18 | `notificationsService.ts` | ✅ MIGRADO | 5 calls. Logger prefixado |
| 19 | `auditService.ts` | ✅ MIGRADO | 1 call. Import supabase removido |
| 20 | `healthService.ts` | ✅ MIGRADO | 5 calls. invokeEdgeFunction preservado |
| 21 | `confirmedRepairService.ts` | ✅ MIGRADO | 4 calls. EX-001 (auth) |
| 22 | `serviceIntentsService.ts` | ✅ MIGRADO | 3 calls. eslint-disable→tipo explícito |
| 23 | `crossOsPatternsService.ts` | ✅ MIGRADO | 1 RPC sem tipo → queryService.rpc+cast. EX-012 |
| 24 | `biService.ts` | ✅ MIGRADO | 4 calls (views). Logger adicionado |
| 25 | `youtubeService.ts` | ✅ MIGRADO | supabase.functions.invoke→invokeEdgeFunction |
| 26 | `supplierService.ts` | ✅ JÁ CORRETO | 2 eslint-disable as any → Record<string,unknown> |
| 27 | `symptomService.ts` | ✅ MIGRADO | 8 calls. eslint-disable→Record<string,unknown> |
| 28 | `odometerCorrectionsService.ts` | ✅ MIGRADO | 4 calls + 2 RPCs. Inline desmembrado |
| 29 | `diagnosticAnalyticsService.ts` | ✅ MIGRADO | supabase.rpc→queryService.rpc |

---

### ✅ ONDA 5 — CONCLUÍDA

**SHA:** `6d500b3` | **Data:** 2026-03-18 | **build:** exit 0

- 39 pages migradas de `console.*` → `logger.error/info/warn`
- ~120 ocorrências substituídas
- `import { logger }` adicionado em todos os arquivos
- **Zero violações arquiteturais detectadas** nas pages

---

### ✅ ONDA 6A — CONCLUÍDA (Limpeza técnica — components + auditoria)

**SHA:** `aeb7c3f` | **Data:** 2026-03-18 | **build:** exit 0

- 20 components migrados de `console.*` → `logger.error/info/warn` (29 ocorrências)
- `import { logger }` adicionado em todos os componentes
- **ZERO console.* em toda a camada UI web (pages + components)** — D-003 expandida
- R-002 investigado: Dependabot sem PR — requer Owner
- R-003 investigado: `test-pdf` sem consumidores — remoção requer Owner
- R-004 confirmado: 2 migrations com timestamp igual — renomeação requer Owner

---

### ▶️ ONDA 7 — OBD Knowledge Base (EM ANDAMENTO)

#### ✅ Fase 1 — RPCs fantasma

**SHA:** `f1b6ce9` | **Data:** 2026-03-18 | **tsc:** 0 erros

- 5 RPCs criadas: `get_knowledge_stats`, `search_knowledge`, `search_knowledge_fulltext`, `ingest_knowledge_from_repairs`, `get_cross_os_patterns`
- Migration: `20260318000000_knowledge_engine_rpcs.sql`
- Nenhuma tabela nova — contratos derivados de `diagnostic_evidence` + `diagnostic_hypotheses` + `confirmed_repairs`
- CHECK constraint expandido com `confirmed_repair_digest`

#### ✅ Fase 2 — Regenerar tipos + limpar casts (SHA `5e61f02`)

| Item | Status |
|------|--------|
| Regenerar `database.ts` via `generate_typescript_types` | ✅ Executado — RPCs PL/pgSQL retornam jsonb, gerador não cria tipagem automática. queryService.rpc() genérico resolve. |
| Remover casts `as unknown as ...` de `knowledgeEngineService.ts` | ✅ Concluído |
| Remover casts de `crossOsPatternsService.ts` | ✅ Concluído |
| tsc --noEmit | ✅ 0 erros |
| npm run build | ✅ exit 0 |

#### ✅ Fase 4 — knowledge_rules Amadurecido (SHA `23ad8c3`)

| Item | Status |
|------|--------|
| `updated_at` + trigger | ✅ Migration aplicada |
| Tipagem web corrigida (`as any` removido) | ✅ Concluído |
| D-013: catálogo global (sem `company_id`) | ✅ Decisão formal |
| tsc --noEmit | ✅ 0 erros |
| npm run build | ✅ exit 0 |

#### 📋 Fase 5+ — Decisões pendentes (Owner)

| Frente | Decisão necessária |
|---------|-------------------|
| CRUD `knowledge_rules` admin web | Definir se será web-only ou web+mobile |
| ~~Multi-tenant `knowledge_rules`~~ | ✅ D-013 — catálogo global (sem `company_id`) |
| ~~Unificação `analisar-dtc` vs `dtc_analyze`~~ | ✅ D-012 — `analisar-dtc` canônica, `dtc_analyze` legado (Rodada 13) |
| OBD Center R4/R5 (mobile) | Integração mobile com Knowledge Base |

---

## 7. HISTÓRICO DE RODADAS

| Rodada | SHA | Data | O que foi feito | build |
|--------|-----|------|-----------------|:---:|
| 14 — ONDA 7F4 | `23ad8c3` | 2026-03-18 | updated_at + trigger. Tipagem web corrigida. D-013 catálogo global. | ✅ |
| 13 — ONDA 7F3 DTC | `780b5e6` | 2026-03-18 | D-012: analisar-dtc canônica, dtc_analyze legado. Mobile migrado. Banner legado. | ✅ |
| 12 — Validação ONDA 7 | `7192854` | 2026-03-18 | 5/5 RPCs testadas no Supabase. 2/2 services OK. 2/2 dashboards HTTP 200. tsc 0 erros. build exit 0. Zero código alterado. | ✅ |
| 11 — Limpeza Doc | `477af2a` | 2026-03-18 | 15 depreciados, 2 atualizados, 3 removidos. R-009/R-010/R-011 resolvidos. D-011 registrada. | ✅ |
| 10 — Auditoria Doc | `f748be1` | 2026-03-18 | 250+ docs mapeados. 18 legado. 4 conflitos. Hierarquia 5 camadas proposta. Zero alterações. | ✅ |
| 9 — ONDA 7F2 | `689c529` | 2026-03-18 | Casts removidos de knowledgeEngineService + crossOsPatternsService. EX-011/EX-012 resolvidos. | ✅ |
| 8B — Integração Doc | `2c44e1e` | 2026-03-18 | GEMINI.md §16 + STATUS.md + AGENT_MASTER_CONTROL + 07_GOVERNANCA + COMANDOS_RAPIDOS integrados. Hierarquia D-004/D-005 | ✅ |
| 8 — ONDA 7F1 | `f1b6ce9` | 2026-03-18 | 5 RPCs fantasma criadas. Knowledge Engine + Cross-OS desbloqueados. | ✅ |
| 7 — ONDA 6A | `aeb7c3f` | 2026-03-18 | 20 components: console.*→logger. 29 ocorrências. UI 100% limpa. | ✅ |
| 6 — ONDA 5 | `6d500b3` | 2026-03-18 | 39 pages: console.*→logger. ~120 ocorrências. Zero violações. | ✅ |
| 5 — ONDA 4 | `3763e8e` | 2026-03-17 | 14 services suporte migrados. EX-011+EX-012. | ✅ |
| 4 — ONDA 3 | `e53fc55` | 2026-03-17 | 4 services migrados + towService confirmado correto | ✅ |
| 3 — GEMINI.md Seção 18 | `a0e75b8` | 2026-03-17 | STATUS como memória permanente formalizada | ✅ |
| 3 — STATUS reestruturado | `42687ba` | 2026-03-17 | 10 seções, contratos, exceções, decisões, riscos | ✅ |
| 3 — ONDA 2 | `7efaf5e` | 2026-03-17 | appointmentService migrado + 5 auditados | ✅ |
| 2 — ONDA 1 | `599bff4` | 2026-03-17 | 4 services críticos migrados (63+ calls) | ✅ |
| 1 — Barramento | `3507fc1` | 2026-03-17 | Repo público + Seção 17 GEMINI.md | ✅ |
| — | `dd67fd0` | 2026-03-17 | Hardening A: 5 services | ✅ |
| — | `2f5967f` | 2026-03-17 | WhatsApp Inbox: 4 drifts corrigidos | ✅ |

---

## 8. DECISÕES FORMAIS REGISTRADAS

> Decisões que não podem ser revertidas sem aprovação do Owner.

| ID | Decisão | Data | SHA referência |
|----|---------|------|----------------|
| D-001 | Catálogo de MO, Peças, Cotações e Financeiro são **web-only** | 2026-03-17 | — |
| D-002 | queryService é o único ponto de acesso a tabelas. supabase direto banido em UI (pages/hooks/components) | 2026-03-17 | `599bff4` |
| D-003 | logger é o único ponto de log. console.* banido em **services, pages e components** web | 2026-03-18 | `aeb7c3f` |
| D-004 | Modo Evolução de Produto ativo desde 2026-03-16. Saída do modo estabilização formal. | 2026-03-16 | — |
| D-005 | Multi-role implementado. ROLE_PERMISSION_MATRIX é a fonte canônica de permissões | 2026-03-17 | — |
| D-006 | WhatsApp modal de aprovação usa short OS codes (não heurísticas). Hardening completado | 2026-03-13 | `2f5967f` |
| D-007 | Repo público YouAutoCar-Status é o canal oficial ChatGPT→Antigravity | 2026-03-17 | `3507fc1` |
| D-008 | Hierarquia documental formalizada: GEMINI.md=lei, STATUS.md=memória, 10_AI_STRATEGY=direção, .agents/=operação | 2026-03-18 | `2c44e1e` |
| D-009 | `docs/10_AI_STRATEGY/` é fonte **obrigatória** de contexto antes de features IA (GEMINI.md §16) | 2026-03-18 | `2c44e1e` |
| D-010 | **Autoridade documental formalizada**: `docs/AI/` inteira é LEGADO — nenhum agente deve usar como fonte de decisão. `00_START_HERE.md` é LEGADO. Fontes confiáveis: GEMINI.md + STATUS.md + 10_AI_STRATEGY + .agents/ | 2026-03-18 | Rodada 10 |
| D-011 | **Limpeza documental executada**: 15 banners ⚠️ LEGADO aplicados, 2 docs atualizados (global_rules, SERVICES_AND_DATA_ACCESS), 3 lixos removidos. Hierarquia de 5 camadas efetivada. | 2026-03-18 | Rodada 11 |
| D-012 | **`analisar-dtc` é a função canônica** para análise DTC. `dtc_analyze` é legado V1 (mantido apenas como fallback web). Mobile totalmente migrado. | 2026-03-18 | `780b5e6` |
| D-013 | **`knowledge_rules` é catálogo global** — sem `company_id`. Regras técnicas universais. Se empresa precisar de regras custom, será tabela separada. | 2026-03-18 | `23ad8c3` |

---

## 9. RISCOS RESIDUAIS ABERTOS

| ID | Risco | Gravidade | Status | Onda |
|----|-------|:---------:|--------|:----:|
| R-001 | `supabase.rpc('import_quotation_prices')` sem tipo gerado — risco de drift silencioso | MÉDIO | EX-005 aceito | Após regen de tipos |
| R-002 | 3 Dependabot alerts (1 critical, 1 high, 1 moderate) | ALTO | Investigado — sem PR automático. Owner decide atualização de deps | Owner |
| R-003 | Edge Function `test-pdf` em produção sem consumidores | MÉDIO | Investigado — zero referências. Remoção requer Owner | Owner |
| R-004 | 2 migrations com timestamp `20260310000000` (`auto_dispatch_engine` + `fix_legacy_br_constraints`) | MÉDIO | Confirmado — renomeação requer Owner | Owner |
| R-005 | ~~50+ console.* em pages web~~ | ~~BAIXO~~ | ✅ RESOLVIDO — ONDA 5 | — |
| R-006 | ~~`diagnosticUploadService` + `evidenceService` com supabase.storage direto~~ | ~~BAIXO~~ | ✅ RESOLVIDO — EX-009 (ONDA 3) | — |
| R-007 | ~~OBD Knowledge Base sem auditoria prévia~~ | ~~MÉDIO~~ | ✅ RESOLVIDO — Auditoria + Fase 1 CONCLUÍDAS | ONDA 7 |
| R-008 | ~~20 components web com console.* residual~~ | ~~BAIXO~~ | ✅ RESOLVIDO — ONDA 6A | — |
| R-009 | ~~18 documentos legado sem banner~~ | ~~ALTO~~ | ✅ RESOLVIDO — Rodada 11. 15 banners ⚠️ LEGADO aplicados. | — |
| R-010 | ~~global_rules.md §5 com milestones antigos e AI_CONTEXT_SNAPSHOT~~ | ~~ALTO~~ | ✅ RESOLVIDO — Rodada 11. §5 migrado para ONDAs + STATUS.md. | — |
| R-011 | ~~SERVICES_AND_DATA_ACCESS.md com supabase.from() legado~~ | ~~MÉDIO~~ | ✅ RESOLVIDO — Rodada 11. Exemplo corrigido para queryService.from(). | — |

---

## 10. PROMPT SUGERIDO PARA PRÓXIMA SESSÃO

> Copie o bloco abaixo integralmente e envie ao Antigravity.
> Não modifique — o prompt foi calibrado para ativar o protocolo correto.

---

### ✅ HIERARQUIA DOCUMENTAL EFETIVADA (D-011)

| Camada | Escopo | Arquivos |
|--------|--------|----------|
| **1 — Governança** (LEI) | Manda em tudo | `GEMINI.md`, `STATUS.md`, `IDENTITY_CONTRACT`, `ROLE_PERMISSION_MATRIX` |
| **2 — Direção Estratégica** | Visão IA | `docs/10_AI_STRATEGY/*` |
| **3 — Referência** | Consulta | `docs/02_PRODUCT/`, `docs/03_DB/`, contratos canônicos |
| **4 — Execução** | Agentes/Operação | `.agents/rules/`, `.agents/skills/`, `.agents/workflows/` |
| **❌ Legado** (NÃO guia) | Banner ⚠️ | `docs/AI/*`, `00_START_HERE`, `architecture_rules`, `AI_DEVELOPMENT_WORKFLOW` |

### ⚠️ FONTES CONFIÁVEIS (D-010)

| `GEMINI.md` | `docs/AI/*` (toda a pasta — banner ⚠️ LEGADO) |
| `docs/audit/STATUS.md` | `docs/00_START_HERE.md` (banner ⚠️ LEGADO) |
| `docs/10_AI_STRATEGY/*` | `docs/01_GOVERNANCE/AI_DEVELOPMENT_WORKFLOW.md` (banner ⚠️ LEGADO) |
| `.agents/rules/AGENT_MASTER_CONTROL.md` | `docs/01_GOVERNANCE/architecture_rules.md` (banner ⚠️ LEGADO) |
| `docs/01_GOVERNANCE/ROLE_PERMISSION_MATRIX.md` | |
| `docs/03_DB/CANONICAL_CONTRACTS.md` | |
| `.agents/rules/global_rules.md` (atualizado R11) | |
| `docs/01_GOVERNANCE/SERVICES_AND_DATA_ACCESS.md` (atualizado R11) | |

---

### Contexto para próxima sessão

**Rodada 14 — ONDA 7 Fase 4 Concluída:**
- ✅ `updated_at` + trigger adicionados a `knowledge_rules`
- ✅ Tipagem web corrigida (`eslint-disable as any` removido)
- ✅ D-013: knowledge_rules é catálogo global (sem company_id)
- ✅ tsc 0 erros | build exit 0

**Próxima frente — ONDA 7 Fase 5+ (código):**
```
Antigravity, ONDA 7 Fases 1-4 concluídas (Rodada 14).

knowledge_rules amadurecido: updated_at, trigger, tipagem, decisão global (D-013).

Decisões Owner necessárias para prosseguir:
1. CRUD knowledge_rules admin web — construir tela de gestão?
2. OBD Center R4/R5 mobile — integração com Knowledge Base
```

### Itens Owner (paralelos):
1. **Dependabot alerts** — 1 critical, 1 high, 1 moderate
2. **Edge Function `test-pdf`** — remover?
3. **Migrations duplicadas** `20260310000000` — renomear?
4. **CRUD knowledge_rules admin** — decisão de escopo
5. **Fase 3C futura** — avaliar remoção do fallback `dtc_analyze` no web

---
