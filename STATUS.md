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
4. [CONTRATOS ARQUITETURAIS](#4-contratos-arquiteturais)
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
- Antigravity atualiza os dois STATUS.md ao final de cada rodada, salvo instrução do Owner para adiar
- ChatGPT NUNCA sugere implementações fora do plano documentado
- Owner é o único que pode autorizar desvios de escopo
- Qualquer fato arquitetural novo descoberto durante a rodada vai direto ao STATUS.md

**URL para leitura pelo ChatGPT (raw, sem autenticação):**
```
https://raw.githubusercontent.com/You-Telecom-Provedor-de-internet/YouAutoCar-Status/main/STATUS.md
```



## 2. ESTADO ATUAL

| Campo | Valor |
|-------|---------|
| **Rodada** | 28 |
| **SHA código** | `8ce7740` |
| **SHA status** | `8ce7740` |
| **Data** | 2026-03-19 |
| **Modo** | EVOLUÇÃO DE PRODUTO — Auditoria Mestra V1 (modo supervisionado) |
| **tsc** | ✅ 0 erros |
| **build** | ✅ exit 0 |
| **flutter analyze** | ✅ 0 erros |
| **ONDA ativa** | Auditoria Mestra V1 — módulo por módulo |
| **Próxima ação obrigatória** | Módulo 7 — próximo domínio conforme mapa §3.2 |

### Resumo das últimas rodadas

**Rodada 26 — Auditoria Mestra V1 (Módulos 1–4):**
- ✅ M1(OS): eslint-disable→8 pontuais. M2(Diagnóstico): 0 correções. M3(Clientes): clientPortalService migrado queryService. M4(Frota): 0 correções.
- ✅ GEMINI.md §2.1: ALL Turbo → supervisionado

**Rodada 27 — Auditoria Mestra V1 (Módulo 5 — Agendamentos):**
- ✅ 13/13 fluxos validados com browser tests (CRUD, conversão OS, status, delete)
- ✅ 0 violações arquiteturais. 0 correções necessárias.

**Rodada 28 — Auditoria Mestra V1 (Módulo 6 — Financeiro):**
- ✅ 3 bugs corrigidos: company_id destructuring, queryService.auth proxy, companyService canonical
- ✅ 2 migrations: trigger trg_sync_os_payment_to_financial (customer_id + SECURITY DEFINER), RLS financial_transactions_member_insert
- ✅ 1 fix frontend: toast [object Object] → PostgrestError.message
- ✅ 5 browser tests OK (dashboard, tabs, criar transação, pagamento OS)
- ✅ GEMINI.md §17 adicionado: atualização STATUS.md obrigatória por rodada
- ⚠️ **Sem commit/push** nesta rodada — aguardando instrução do Owner
- ✅ `tsc --noEmit` = 0 erros | `npm run build` = exit 0

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
| Ordens de Serviço | 🟢 | 🟢 | **Módulo 1 V1 ✅** — eslint-disable global removido, @ts-expect-error→cast |
| Clientes | 🟢 | 🟢 | **Módulo 3 V1 ✅** — clientPortalService migrado queryService (7 queries) |
| Veículos | 🟢 | 🟢 | **Módulo 4 V1 ✅** — 12/12 fluxos OK. 0 violações |
| Agendamentos | 🟢 | 🟢 | **Módulo 5 V1 ✅** — 13/13 fluxos + browser tests (CRUD, OS, status, delete). 0 violações |
| Financeiro | 🟢 | 🟢 | **Módulo 6 V1 ✅** — 3 bugs corrigidos (company_id, auth, companyService). Browser test OK. DRE + KPIs |
| Catálogo MO | 🟢 | 🟢 | ONDA 2 ✅ — laborCatalogService já correto |
| Peças | 🟢 | 🟢 | ONDA 2 ✅ + ONDA 5 ✅ — service correto. console.* removido |
| Pedidos de Compra | 🟢 | 🟢 | ONDA 2 ✅ + ONDA 5 ✅ — service correto. console.* removido |
| Cotações | 🟢 | 🟢 | ONDA 2 ✅ + ONDA 5 ✅ — service correto. EX-005 (RPC). console.* removido |
| WhatsApp | 🟢 | 🟢 | Hardening completo. 4 drifts de campo corrigidos |
| OBD / DTCs | 🟢 | 🟢 | **Módulo 2 V1 ✅** — 5 EFs Gemini, 10/10 fluxos. Pipeline completo |
| Diagnóstico (service) | 🟢 | 🟢 | **Módulo 2 V1 ✅** — EX-001, EX-009, EX-010 documentados |
| Diagnóstico Upload (service) | 🟢 | 🟢 | ONDA 3 ✅ — migrado. EX-001, EX-009 (scan-reports bucket) |
| Hipóteses (service) | 🟢 | 🟢 | ONDA 3 ✅ — migrado. import supabase removido |
| Evidências (service) | 🟢 | 🟢 | ONDA 3 ✅ — migrado. EX-001, EX-003, EX-009 |
| Guincho/Tow | 🟢 | 🟢 | ONDA 3 ✅ — já correto. queryService em todo o arquivo |
| Knowledge Engine | 🟢 | 🟢 | ONDA 7 ✅ — migrado. ~~EX-011~~ resolvido Fase 2 |
| Scanner Context | 🟢 | 🟢 | ONDA 4 ✅ — migrado. 3 tabelas sem tipo → interface local |
| Notificações | 🟢 | 🟢 | ONDA 4 ✅ + R23 ✅ — identity drift triggers corrigido (`auth_id` via JOIN) |
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
| Edge Functions | 🟢 | — | 32 no repo (excluindo _shared). 46 deployadas no Supabase (inclui legado: dtc_analyze, bootstrap_admin, etc). Convenção: contar pelo repo. |
| RLS / Banco | 🟢 | — | 167+ migrations. Multi-role implementado. ROLE_PERMISSION_MATRIX vigente |
| CI/CD | 🟡 | — | **Desativado temporariamente** (D-016). Workflows intactos. vite restaurado R21. Validação local obrigatória |
| **IA Strategy** (`docs/10_AI_STRATEGY/`) | 🟢 | — | 8 docs integrados na governança (GEMINI.md §16). `07_GOVERNANCA` atualizado. |
| **Agents/Skills** (`.agents/`) | 🟢 | — | 9 skills + 62 workflows + 5 AGE ok. AGENT_MASTER_CONTROL e COMANDOS_RAPIDOS ✅ atualizados R8B |

### 3.3 Domínios Mobile

| Domínio | Status | Observação |
|---------|:---:|-----------|
| Login / Auth | 🟢 | Riverpod + Supabase Auth ok |
| OBD ELM327 | 🟢 | Bluetooth RFCOMM. ELM327 commands ok |
| OBD Center (UI) | 🟢 | R1–R5 completos. R4: Knowledge insights por DTC. R5: KnowledgeInsightsCard |
| Viagens / Telemetria | 🟢 | Trip sessions ok |
| Alertas OBD | 🟢 | Engine ok. Thresholds por perfil |
| Catálogo MO | 🔴 | Web-only — decisão formal |
| Peças | 🔴 | Web-only — decisão formal |
| Cotações | 🔴 | Web-only — decisão formal |
| Financeiro | 🔴 | Web-only — decisão formal |
| WhatsApp | N/A | Apenas backend/web |

---

## 4. CONTRATOS ARQUITETURAIS

> Regras e contratos completos: ver **GEMINI.md §5** (regras proibitivas), **§7–8** (AGE), **§9** (DoD).
> Esta seção não repete a lei — apenas referencia.

---

## 5. EXCEÇÕES ARQUITETURAIS VIGENTES

> Estas são as únicas exceções aceitas ao Contrato 4.1.
> Qualquer nova exceção deve ser adicionada aqui (única fonte de verdade — GEMINI.md §6 aponta para esta tabela).

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

### Ondas Concluídas (resumo)

| Onda | SHA | Data | Escopo |
|------|-----|------|--------|
| ONDA 1 | `599bff4` | 2026-03-17 | 4 services críticos migrados (63+ calls) |
| ONDA 2 | `7efaf5e` | 2026-03-17 | 6 services auditados (appointment migrado, 5 já corretos) |
| ONDA 3 | `e53fc55` | 2026-03-17 | 5 services diagnóstico migrados |
| ONDA 4 | `3763e8e` | 2026-03-17 | 14 services suporte migrados |
| ONDA 5 | `6d500b3` | 2026-03-18 | 39 pages: console.* → logger (~120 ocorrências) |
| ONDA 6A | `aeb7c3f` | 2026-03-18 | 20 components limpos. ZERO console.* na UI web |
| ONDA 7 | `3516046` | 2026-03-18 | OBD Knowledge Base: 5 RPCs, tipos regenerados, CRUD web, R4/R5 mobile |

---

## 7. HISTÓRICO DE RODADAS

| Rodada | SHA | Data | O que foi feito | build |
|--------|-----|------|-----------------|:---:|
| 25 — Fix Pipeline parse-scan-pdf | `efb0193` | 2026-03-19 | CORS fix (x-request-id). Reescrita EF: imports dinâmicos, fallback, createLogger. v12→v16. DTCs P0325+P0123 extraídos Napro.pdf ✅ | ✅ |
| 24C — Fix Catálogo Peças+Serviços | `6152a84` | 2026-03-19 | BUG-1: schema drift parts_catalog (default_price→average_price). BUG-2: .maybeSingle()→.limit(1) para multi-membership. Ambos validados via browser | ✅ |
| 24 — Schema Drift + Bug 400 Checklist | `4d7f62d` | 2026-03-19 | 12 correções: 3 schema drift (diagnosticService), 2 tabela/EF errada (DiagnosticsTab), msg enganosa (diagnosticUploadService), 4 console→logger (useDiagnostics), bug 400 Checklist (categorias+status inválidos), CHECKLIST_STATUS_CONFIG migrado para pass/warn/fail | ✅ |
| 23 — Fix Identity Drift Triggers | `dac2269` | 2026-03-19 | FK violation 23503 corrigida: 2 triggers (notify_on_os_created, notify_on_os_status_change) usavam public.users.id em vez de auth.users.id. Toast [object Object] fix (3 arquivos). OS criada com sucesso. | ✅ |
| 22 — Auditoria Funcional OS | `19ce537` | 2026-03-19 | 3 bugs OS corrigidos: identity drift customerService, contrato NewClientModal, contrato NewVehicleModal. Validação via navegador. | ✅ |
| 21 — Auditoria CI/CD | `f6cc561` | 2026-03-19 | Auditoria 231 runs. Billing block (P-001 Owner). vite restaurado (P-002). Workflows ok. | ✅ |
| 20 — Hotfix tela branca | `81c79e2` | 2026-03-19 | Fix import type UseFormReturn (ESM hang). Remove landingRedirect plugin. Landing+Login ok. | ✅ |
| 19 — CRUD knowledge_rules | `88017a0` | 2026-03-18 | CRUD admin web-only. knowledgeEngineService (4 métodos). KnowledgeRulesTab.tsx. Dashboard com Tabs. | ✅ |
| 18 — Remoção dtc_analyze | `9afc200` | 2026-03-18 | EF dtc_analyze removida do repo. Zero consumidores. analisar-dtc canônica. R-012 resolvido. | ✅ |
| 17 — Limpeza Legado | `2f1c8fa` | 2026-03-18 | 8 docs/AI → _archive. Fallback OpenAI removido. global_rules §10.1 corrigido. dtc_analyze DEPRECATED. | ✅ |
| 16 — Limpeza Owner | `43bd6b6` | 2026-03-18 | Dependabot 3 alerts fix (jspdf+esbuild). test-pdf deletado. Migration renomeada. | ✅ |
| 15 — ONDA 7F5 R4/R5 | `3516046` | 2026-03-18 | OBD Center R4/R5: Knowledge insights + KnowledgeInsightsCard. Zero estruturas novas. | ✅ |
| 14 — ONDA 7F4 | `23ad8c3` | 2026-03-18 | updated_at + trigger. Tipagem web corrigida. D-013 catálogo global. | ✅ |
| 13 — ONDA 7F3 DTC | `780b5e6` | 2026-03-18 | D-012: analisar-dtc canônica, dtc_analyze legado. Mobile migrado. Banner legado. | ✅ |
| 12 — Validação ONDA 7 | `7192854` | 2026-03-18 | 5/5 RPCs testadas no Supabase. 2/2 services OK. 2/2 dashboards HTTP 200. tsc 0 erros. build exit 0. Zero código alterado. | ✅ |
| 11 — Limpeza Doc | `477af2a` | 2026-03-18 | 15 depreciados, 2 atualizados, 3 removidos. R-009/R-010/R-011 resolvidos. D-011 registrada. | ✅ |
| 10 — Auditoria Doc | `f748be1` | 2026-03-18 | 250+ docs mapeados. 18 legado. 4 conflitos. Hierarquia 5 camadas proposta. Zero alterações. | ✅ |
| 9 — ONDA 7F2 | `689c529` | 2026-03-18 | Casts removidos de knowledgeEngineService + crossOsPatternsService. EX-011/EX-012 resolvidos. | ✅ |
| 8B — Integração Doc | `2c44e1e` | 2026-03-18 | GEMINI.md §16 + STATUS.md + AGENT_MASTER_CONTROL + 07_GOVERNANCA + COMANDOS_RAPIDOS integrados. Hierarquia D-004/D-005 | ✅ |
| 8 — ONDA 7F1 | `f1b6ce9` | 2026-03-18 | 5 RPCs fantasma criadas. Knowledge Engine + Cross-OS desbloqueados. | ✅ |
| 7 — ONDA 6A | `aeb7c3f` | 2026-03-18 | 20 components: console.*→logger. 29 ocorrências. UI 100% limpa. | ✅ |

> Rodadas 1–6 (2026-03-17): ONDAs 1–5 + barramento + WhatsApp hardening. Ver §6 para resumo.

| 26 — Auditoria Mestra V1 M1-M4 | `8ce7740` | 2026-03-19 | M1(OS): eslint-disable→8 pontuais. M2(Diagnóstico): 0 correções. M3(Clientes): clientPortalService migrado queryService (7 queries). M4(Frota): 0 correções. GEMINI.md: ALL Turbo→supervisionado | ✅ |
| 27 — Auditoria Mestra V1 M5 | `8ce7740` | 2026-03-19 | M5(Agendamentos): 13/13 fluxos, browser tests (CRUD, OS, status, delete). 0 violações. 0 correções. | ✅ |
| 28 — Auditoria Mestra V1 M6 | `8ce7740` | 2026-03-19 | M6(Financeiro): 3 bugs corrigidos (company_id, auth, companyService). 2 migrations (trigger, RLS). Toast [object Object] fix. 5 browser tests OK. GEMINI.md §17 adicionado. | ✅ |

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
| D-012 | **`analisar-dtc` é a função canônica** para análise DTC. `dtc_analyze` **REMOVIDA** do repo na Rodada 18. Fallback OpenAI removido R17. Mobile nunca usou. Domínio DTC/IA limpo. | 2026-03-18 | Rodada 18 |
| D-013 | **`knowledge_rules` é catálogo global** — sem `company_id`. Regras técnicas universais. Se empresa precisar de regras custom, será tabela separada. | 2026-03-18 | `23ad8c3` |
| D-014 | **`docs/AI/` inteira arquivada em `_archive/`** — Rodada 17. 8 handoffs/snapshots congelados. Não são fonte de decisão. `00_START_HERE.md` refs AI limpas. | 2026-03-18 | Rodada 17 |
| D-015 | **`global_rules.md` §10.1 corrigido** — fontes canônicas: GEMINI.md, STATUS.md, 10_AI_STRATEGY, global_rules. Referências a `AI_CONTEXT_SNAPSHOT` e `ARCHITECTURE_OVERVIEW` removidas. | 2026-03-18 | Rodada 17 |
| D-016 | **GitHub Actions desativado temporariamente** para controle de custos. Workflows intactos (não deletar). Validação obrigatoriamente local: `tsc --noEmit`, `npm run build`, `flutter analyze`. Reativação depende do Owner. | 2026-03-19 | Rodada 21 |

---

## 9. RISCOS RESIDUAIS ABERTOS

> Apenas riscos ativos. Riscos resolvidos foram arquivados no histórico de rodadas.

| ID | Risco | Gravidade | Status |
|----|-------|:---------:|--------|
| R-001 | `supabase.rpc('import_quotation_prices')` sem tipo gerado — risco de drift silencioso | MÉDIO | EX-005 aceito |
| R-CI-001 | GitHub Actions desativado (D-016) — validações devem ser locais | ACEITO | ⚠️ Desativação controlada por custos |
| R-CI-003 | Watchdog inoperante enquanto CI desativado | ACEITO | ⚠️ Reativar junto com CI |
| R-FIN-001 | `financialService.createTransactionWithAuth` usa `fetch` direto para EF `create_recurring_transactions` em vez de `queryService.invoke` | BAIXO | Funcional, não bloqueia V1 |

---

## 10. PROMPT SUGERIDO PARA PRÓXIMA SESSÃO

> Copie o bloco abaixo integralmente e envie ao Antigravity.
> Não modifique — o prompt foi calibrado para ativar o protocolo correto.

---

### Contexto para próxima sessão

**Rodadas 26–28 — Auditoria Mestra V1 (Módulos 1–6):**
- ✅ 6 módulos auditados e aprovados para V1
- ✅ Módulo 6 Financeiro: 3 bugs corrigidos (company_id, auth, companyService) + 2 migrations (trigger, RLS)
- ✅ GEMINI.md §17 adicionado: atualização STATUS.md obrigatória por rodada
- ✅ Build OK, tsc 0 erros
- ⚠️ Sem commit/push — aguardando instrução Owner

**Próxima frente sugerida:**
```
Antigravity, iniciar o MÓDULO 7 da AUDITORIA MESTRA DE FINALIZAÇÃO DA V1.0.

Rodadas anteriores fecharam 6 módulos:
1. OS ✅ | 2. Diagnóstico ✅ | 3. Clientes ✅ | 4. Frota ✅ | 5. Agendamentos ✅ | 6. Financeiro ✅

Próximo módulo a auditar conforme mapa de domínios do STATUS.md.
Browser test obrigatório para V1.
```

### Itens Owner (paralelos):
1. **Commit/Push** — aprovar ou adiar commit das Rodadas 26–28
2. **Billing GitHub** — resolver pagamento para reativar CI/CD
3. **Sync STATUS.md público** — sincronizar `c:\www\YouAutoCar-Status\STATUS.md` se houver commit

---
