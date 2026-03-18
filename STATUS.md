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
| **Rodada** | 4 |
| **SHA código** | `e53fc55` |
| **SHA status** | `16e0c58` |
| **Data** | 2026-03-17 |
| **Modo** | MISSÃO MESTRA — Fechamento Total (Zero Dívida Técnica Oculta) |
| **tsc --noEmit** | ✅ 0 erros |
| **ONDA ativa** | ONDA 4 (14 services de suporte) |
| **Próxima ação obrigatória** | Executar ONDA 4 |

### Resumo da última rodada

**ONDA 3 executada:**
- 5 services auditados: `diagnosticService`, `diagnosticUploadService`, `hypothesisService`, `evidenceService`, `towService`
- `towService.ts`: **JÁ CORRETO** — queryService em todo o arquivo
- `diagnosticService.ts`: **MIGRADO** — 15+ calls `supabase.from()` → `queryService`. EX-001 (auth), EX-009 (storage scan-reports), EX-010 (RPC delete_diagnostic_entry)
- `diagnosticUploadService.ts`: **MIGRADO** — 8 calls `supabase.from()` → `queryService`. EX-001, EX-009 (storage scan-reports upload)
- `hypothesisService.ts`: **MIGRADO** — 6 calls. Import `supabase` removido completamente
- `evidenceService.ts`: **MIGRADO** — 5 calls `supabase.from()` → `queryService`. EX-001, EX-003 (functions.invoke analyze_upload), EX-009 (storage diagnostic-evidence upload/signedUrl/remove)
- Novas exceções documentadas: EX-009 (storage — 3 buckets), EX-010 (RPC delete_diagnostic_entry)
- `eslint-disable @ts-expect-error` residuais eliminados — substituídos por `as Record<string, unknown>`

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
| Agendamentos | 🟢 | 🟡 | ONDA 2 ✅ — appointmentService migrado. Pages: console.* residual |
| Financeiro | 🟢 | 🟡 | ONDA 2 ✅ — financialService já correto. Pages: console.* residual |
| Catálogo MO | 🟢 | 🟢 | ONDA 2 ✅ — laborCatalogService já correto |
| Peças | 🟢 | 🟡 | ONDA 2 ✅ — partService já correto. Pages: console.* residual |
| Pedidos de Compra | 🟢 | 🟡 | ONDA 2 ✅ — purchaseOrderService já correto |
| Cotações | 🟢 | 🟡 | ONDA 2 ✅ — quotationService já correto. Exceção RPC documentada |
| WhatsApp | 🟢 | 🟢 | Hardening completo. 4 drifts de campo corrigidos (Rodada histórica) |
| OBD / DTCs | 🟢 | 🟢 | Pipeline de IA restaurado. Evidence + Upload FASE 0-4 ok |
| Diagnóstico (service) | 🟢 | 🟡 | ONDA 3 ✅ — migrado. EX-001, EX-009, EX-010 documentadas |
| Diagnóstico Upload (service) | 🟢 | 🟡 | ONDA 3 ✅ — migrado. EX-001, EX-009 (scan-reports bucket) |
| Hipóteses (service) | 🟢 | 🟡 | ONDA 3 ✅ — migrado. import supabase removido |
| Evidências (service) | 🟢 | 🟡 | ONDA 3 ✅ — migrado. EX-001, EX-003, EX-009 (3 ops storage) |
| Guincho/Tow | 🟢 | 🟡 | ONDA 3 ✅ — já correto. queryService em todo o arquivo |
| Dashboard/Analytics | 🟡 | 🟡 | console.* em pages. biService ONDA 4 pendente |
| Configurações | 🟡 | 🟡 | Settings.tsx: 5 console.error residuais |
| Observabilidade | 🟡 | 🟡 | 50+ console.* em 15+ pages web |
| Edge Functions | 🟢 | — | 34/35 fechadas. test-pdf em produção (remoção: ONDA 6) |
| RLS / Banco | 🟢 | — | 167 migrations. Multi-role implementado. ROLE_PERMISSION_MATRIX vigente |
| CI/CD | 🟢 | — | ci.yml + watchdog.yml verdes |

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
| EX-003 | `supabase.functions.invoke()` | customerService, serviceOrderDetailService, vehicleService | Invoca Edge Functions do backend. queryService não cobre functions. | Apenas para Edge Functions conhecidas e documentadas. |
| EX-004 | `supabase.channel()` / `removeChannel()` | serviceOrderDetailService | Realtime nativo. queryService não cobre Realtime. | Apenas para subscription de tabelas em tempo real. |
| EX-005 | `supabase.rpc('import_quotation_prices')` | quotationService | RPC transacional sem tipo gerado. A RPC foi adicionada via migration e ainda não está nos tipos Supabase. | Apenas esta RPC específica. Remover quando tipos forem regenerados. |
| EX-006 | `fetch()` com token | customerService, financialService | Edge Functions públicas que exigem autenticação explícita via Bearer token. | Apenas para `create_customer_crm`, `admin_update_password`, `create_recurring_transactions`. |
| EX-007 | `supabase` em portais públicos | SupplierPortal, CustomerApprovalPortal | Portais sem sessão Supabase Auth — usuário acessa via token de OS. | Apenas `fetch()` a Edge Functions públicas. Sem `.from()`. |
| EX-008 | `resetSupabaseClient()` | Login.tsx | Troca dinâmica de strategy de persistência auth (localStorage ↔ sessionStorage). | Apenas esta função. Sem query direta. |
| EX-009 | `supabase.storage` | diagnosticUploadService (scan-reports), evidenceService (diagnostic-evidence), diagnosticService (scan-reports remove) | Storage nativo — queryService não cobre Storage. | Apenas operações: upload, createSignedUrl, remove. Sem query diretas. |
| EX-010 | `supabase.rpc('delete_diagnostic_entry')` | diagnosticService | RPC transacional sem tipo gerado. By-pass de constraints fortes ao deletar entrada diagnóstica. | Apenas esta RPC. Remover quando tipos forem regenerados. |

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

### ▶️ ONDA 4 — PRÓXIMA (14 services de suporte)

| # | Arquivo |
|---|---------|
| 16 | `knowledgeEngineService.ts` |
| 17 | `scannerContextRecommendationService.ts` |
| 18 | `notificationsService.ts` |
| 19 | `auditService.ts` |
| 20 | `healthService.ts` |
| 21 | `confirmedRepairService.ts` |
| 22 | `serviceIntentsService.ts` |
| 23 | `crossOsPatternsService.ts` |
| 24 | `biService.ts` |
| 25 | `youtubeService.ts` |
| 26 | `supplierService.ts` |
| 27 | `symptomService.ts` |
| 28 | `odometerCorrectionsService.ts` |
| 29 | `diagnosticAnalyticsService.ts` |

---

### 📋 ONDA 5 — Observabilidade em pages web

**Problema:** 50+ `console.*` em 15+ pages. Estimativa:
- `ServiceOrders.tsx` / `ServiceOrderDetail.tsx`
- `Customers.tsx` / `Vehicles.tsx`
- `Appointments.tsx` / `Financial.tsx`
- `Settings.tsx` (5 console.error confirmados)
- `Dashboard.tsx` / `Analytics.tsx`

---

### 📋 ONDA 6 — Limpeza técnica

| Item | Problema | Risco |
|------|---------|-------|
| Edge Function `test-pdf` | Em produção sem uso | MÉDIO — ocupa slot |
| 2 migrations `20260310000000` | Timestamp duplicado | MÉDIO — pode gerar drift |
| README.md repo privado | Possivelmente desatualizado | BAIXO |
| Dependabot alerts (3) | 1 critical, 1 high, 1 moderate | ALTO — investigar |

---

### 📋 ONDA 7 — Frentes de produto

| Frente | Pré-requisito | Estimativa |
|--------|--------------|-----------|
| OBD Knowledge Base + Profiles + Rules + Painel | Seção 16 GEMINI.md obrigatória | Grande |
| OBD Center R4/R5 (mobile) | ONDA 7 do plano de produto | Médio |

---

## 7. HISTÓRICO DE RODADAS

| Rodada | SHA | Data | O que foi feito | tsc |
|--------|-----|------|-----------------|:---:|
| 4 — ONDA 3 | `e53fc55` | 2026-03-17 | 4 services migrados + towService confirmado correto | ✅ |
| 3 — GEMINI.md Seção 18 | `a0e75b8` | 2026-03-17 | STATUS como memória permanente formalizada | ✅ |
| 3 — STATUS reestruturado | `42687ba` | 2026-03-17 | 10 seções, contratos, exceções, decisões, riscos | ✅ |
| 3 — ONDA 2 | `7efaf5e` | 2026-03-17 | appointmentService migrado + 5 auditados | ✅ |
| 2 — ONDA 1 | `599bff4` | 2026-03-17 | 4 services críticos migrados (63+ calls) | ✅ |
| 1 — Barramento | `3507fc1` | 2026-03-17 | Repo público + Seção 17 GEMINI.md | ✅ |
| — | `dd67fd0` | 2026-03-17 | Hardening A: 5 services | ✅ |
| — | `2f5967f` | 2026-03-17 | WhatsApp Inbox: 4 drifts corrigidos | ✅ |
| — | `b7ef9f8` | 2026-03-17 | FASE 4: Signed URLs bucket privado | ✅ |
| — | `f794224` | 2026-03-17 | FASE 0+1+2: Evidências Diagnósticas | ✅ |

---

## 8. DECISÕES FORMAIS REGISTRADAS

> Decisões que não podem ser revertidas sem aprovação do Owner.

| ID | Decisão | Data | SHA referência |
|----|---------|------|----------------|
| D-001 | Catálogo de MO, Peças, Cotações e Financeiro são **web-only** | 2026-03-17 | — |
| D-002 | queryService é o único ponto de acesso a tabelas. supabase direto banido em UI (pages/hooks/components) | 2026-03-17 | `599bff4` |
| D-003 | logger é o único ponto de log. console.* banido em services | 2026-03-17 | `dd67fd0` |
| D-004 | Modo Evolução de Produto ativo desde 2026-03-16. Saída do modo estabilização formal. | 2026-03-16 | — |
| D-005 | Multi-role implementado. ROLE_PERMISSION_MATRIX é a fonte canônica de permissões | 2026-03-17 | — |
| D-006 | WhatsApp modal de aprovação usa short OS codes (não heurísticas). Hardening completado | 2026-03-13 | `2f5967f` |
| D-007 | Repo público YouAutoCar-Status é o canal oficial ChatGPT→Antigravity | 2026-03-17 | `3507fc1` |

---

## 9. RISCOS RESIDUAIS ABERTOS

| ID | Risco | Gravidade | Status | Onda |
|----|-------|:---------:|--------|:----:|
| R-001 | `supabase.rpc('import_quotation_prices')` sem tipo gerado — risco de drift silencioso | MÉDIO | EX-005 aceito | ONDA 4 verificar |
| R-002 | 3 Dependabot alerts (1 critical, 1 high, 1 moderate) no repo privado | ALTO | Não investigado | ONDA 6 |
| R-003 | Edge Function `test-pdf` em produção | MÉDIO | Não removida | ONDA 6 |
| R-004 | 2 migrations com timestamp `20260310000000` | MÉDIO | Não verificado | ONDA 6 |
| R-005 | 50+ console.* em pages web | BAIXO | Mapeado | ONDA 5 |
| R-006 | ~~`diagnosticUploadService` e `evidenceService` com supabase.storage direto~~ | ~~BAIXO~~ | ✅ RESOLVIDO — EX-009 documentada (ONDA 3) | — |
| R-007 | OBD Knowledge Base sem auditoria prévia (Seção 16 GEMINI.md obrigatória) | MÉDIO | Bloqueado até auditoria | ONDA 7 |

---

## 10. PROMPT SUGERIDO PARA PRÓXIMA SESSÃO

> Copie o bloco abaixo integralmente e envie ao Antigravity.
> Não modifique — o prompt foi calibrado para ativar o protocolo correto.

---

Antigravity, continuando a Missão Mestra.

Leia o STATUS.md em:
https://raw.githubusercontent.com/You-Telecom-Provedor-de-internet/YouAutoCar-Status/main/STATUS.md

Estado atual: ONDA 3 concluída. SHA base: e53fc55. Rodada 4 iniciando.

Próxima execução: ONDA 4
Auditar e migrar supabase direto para queryService.from() nos 14 services de suporte:
- knowledgeEngineService.ts
- scannerContextRecommendationService.ts
- notificationsService.ts
- auditService.ts
- healthService.ts
- confirmedRepairService.ts
- serviceIntentsService.ts
- crossOsPatternsService.ts
- biService.ts
- youtubeService.ts
- supplierService.ts
- symptomService.ts
- odometerCorrectionsService.ts
- diagnosticAnalyticsService.ts

Protocolo obrigatório:
1. Auditar cada service antes de alterar (Seção 16 GEMINI.md)
2. Listar supabase.from() + console.* por arquivo
3. Migrar para queryService.from() e logger estruturado
4. Preservar exceções corretas: supabase.auth.*, supabase.functions.invoke(), supabase.channel(), supabase.storage (todas são exceções legítimas)
5. Validar tsc --noEmit — 0 erros
6. Commit: refactor(services): ONDA 4 — [descricao]
7. Atualizar docs/audit/STATUS.md no repo privado
8. Atualizar STATUS.md no repo público:
   https://raw.githubusercontent.com/You-Telecom-Provedor-de-internet/YouAutoCar-Status/main/STATUS.md

Formato de entrega:
BLOCO 1 — inventário por service (antes de alterar, 1 linha por arquivo)
BLOCO 2 — arquivos alterados (ou confirmados já corretos)
BLOCO 3 — validação (tsc = 0 erros)
BLOCO 4 — risco residual
BLOCO 5 — commit SHA

---
