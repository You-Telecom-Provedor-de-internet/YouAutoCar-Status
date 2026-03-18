# YouAutoCar — MEMÓRIA OPERACIONAL DO PROJETO
<!-- ARQUIVO GERENCIADO AUTOMATICAMENTE PELO AGENTE ANTIGRAVITY -->
<!-- Este arquivo é a fonte de verdade operacional entre Antigravity, ChatGPT e Owner -->
<!-- URL PÚBLICA (ChatGPT lê aqui sem autenticação): -->
<!-- https://raw.githubusercontent.com/You-Telecom-Provedor-de-internet/YouAutoCar-Status/main/STATUS.md -->

---

## 🔖 ÍNDICE RÁPIDO

1. PROTOCOLO MULTI-AGENTE
2. ESTADO ATUAL
3. MAPA DE DOMÍNIOS
4. ARQUITETURA — CONTRATOS OBRIGATÓRIOS
5. EXCEÇÕES ARQUITETURAIS VIGENTES
6. ONDAS — PLANO DE EXECUÇÃO
7. HISTÓRICO DE RODADAS
8. DECISÕES FORMAIS REGISTRADAS
9. RISCOS RESIDUAIS ABERTOS
10. PROMPT SUGERIDO PARA PRÓXIMA SESSÃO

---

## 1. PROTOCOLO MULTI-AGENTE

| Agente | Papel | O que pode fazer |
|--------|-------|-----------------|
| Antigravity | Execução + Commit | Lê e atualiza STATUS.md, executa código, faz push para ambos os repos |
| ChatGPT | Consultoria + Prompts | Lê STATUS.md via URL pública, sugere próximos prompts ao Owner |
| Owner | Ponte + Decisão | Repassa prompts entre agentes, decide prioridades e escopo |

URL para leitura pelo ChatGPT (raw, sem autenticação):
https://raw.githubusercontent.com/You-Telecom-Provedor-de-internet/YouAutoCar-Status/main/STATUS.md

Regras:
- Antigravity SEMPRE atualiza os dois STATUS.md ao final de cada rodada
- ChatGPT NUNCA sugere implementações fora do plano documentado
- Owner é o único que pode autorizar desvios de escopo
- Qualquer fato arquitetural novo descoberto durante a rodada vai direto ao STATUS.md

---

## 2. ESTADO ATUAL

| Campo | Valor |
|-------|-------|
| Rodada | 3 |
| SHA privado | 37802fe |
| Data | 2026-03-17 |
| Modo | MISSÃO MESTRA — Fechamento Total (Zero Dívida Técnica Oculta) |
| tsc --noEmit | 0 erros |
| ONDA ativa | ONDA 3 (5 services de diagnóstico) |
| Próxima ação | Executar ONDA 3 |

Resumo da última rodada:
ONDA 2 executada. 6 services auditados: financialService, laborCatalogService, partService, purchaseOrderService, quotationService, appointmentService. Descoberta: 5 de 6 já estavam corretos. appointmentService: 7 chamadas supabase.from() migradas para queryService.

---

## 3. MAPA DE DOMÍNIOS

### 3.1 Web

| Domínio | Service | UI | Observação |
|---------|:---:|:---:|-----------|
| Login / Auth | 🟢 | 🟢 | RLS + JWT ok. supabase.auth.* uso legítimo |
| Ordens de Serviço | 🟢 | 🟢 | ONDA 1 — serviceOrderService + detail migrados |
| Clientes | 🟢 | 🟢 | ONDA 1 — customerService migrado |
| Veículos | 🟢 | 🟢 | ONDA 1 — vehicleService migrado |
| Agendamentos | 🟢 | 🟡 | ONDA 2 — appointmentService migrado. Pages: console.* residual |
| Financeiro | 🟢 | 🟡 | ONDA 2 — já correto. Pages: console.* residual |
| Catálogo MO | 🟢 | 🟢 | ONDA 2 — laborCatalogService já correto |
| Peças | 🟢 | 🟡 | ONDA 2 — partService já correto. Pages: console.* residual |
| Pedidos de Compra | 🟢 | 🟡 | ONDA 2 — purchaseOrderService já correto |
| Cotações | 🟢 | 🟡 | ONDA 2 — quotationService correto. EX-005 (RPC sem tipo) |
| WhatsApp | 🟢 | 🟢 | Hardening completo. 4 drifts corrigidos |
| OBD / DTCs / Evidence | 🟢 | 🟢 | Pipeline IA ok. Signed URLs. FASE 0-4 ok |
| Diagnóstico (service) | 🟡 | 🟡 | ONDA 3 pendente |
| Diagnóstico Upload (service) | 🟡 | 🟡 | ONDA 3 pendente — pode ter storage legítimo |
| Hipóteses (service) | 🟡 | 🟡 | ONDA 3 pendente |
| Evidências (service) | 🟡 | 🟡 | ONDA 3 pendente — pode ter storage legítimo |
| Guincho/Tow | 🟡 | 🟡 | ONDA 3 pendente |
| Dashboard/Analytics | 🟡 | 🟡 | console.* em pages. biService ONDA 4 |
| Configurações | 🟡 | 🟡 | Settings.tsx: 5 console.error confirmados |
| Observabilidade pages | 🟡 | 🟡 | 50+ console.* em 15+ pages web |
| Edge Functions | 🟢 | — | 34/35 fechadas. test-pdf em produção (remover ONDA 6) |
| RLS / Banco | 🟢 | — | 167 migrations. Multi-role. ROLE_PERMISSION_MATRIX vigente |
| CI/CD | 🟢 | — | ci.yml + watchdog.yml verdes |

### 3.2 Mobile

| Domínio | Status | Observação |
|---------|:---:|-----------|
| Login / Auth | 🟢 | Riverpod ok |
| OBD ELM327 | 🟢 | Bluetooth RFCOMM ok |
| OBD Center (UI) | 🟡 | R4/R5 pendentes — OBD Knowledge Base |
| Viagens / Telemetria | 🟢 | Trip sessions ok |
| Alertas OBD | 🟢 | Engine ok |
| Catálogo MO | 🔴 | Web-only — decisão D-001 |
| Peças | 🔴 | Web-only — decisão D-001 |
| Cotações | 🔴 | Web-only — decisão D-001 |
| Financeiro | 🔴 | Web-only — decisão D-001 |

---

## 4. ARQUITETURA — CONTRATOS OBRIGATÓRIOS

### Acesso a dados

CAMADA PROIBIDA: pages, components, hooks → supabase.from() NUNCA
CAMADA OBRIGATÓRIA: services → queryService.from() SEMPRE

Exceções aceitas APENAS em services (ver Seção 5):
- supabase.auth.* → identidade
- supabase.functions.* → Edge Functions
- supabase.channel() → Realtime
- supabase.storage → Storage
- fetch() com token → Edge Functions com auth explícita

### Logging

PROIBIDO → OBRIGATÓRIO:
- console.log → logger.info
- console.error → logger.error
- console.warn → logger.warn

### Identidade

PROIBIDO: auth.uid() direto sem helper
OBRIGATÓRIO: get_user_id() ou get_company_id()

### Roles/Permissões

PROIBIDO: role === 'admin' inline
OBRIGATÓRIO: hasPermission() ou ROLE_PERMISSION_MATRIX

---

## 5. EXCEÇÕES ARQUITETURAIS VIGENTES

| ID | Exceção | Arquivo(s) | Justificativa | Escopo |
|----|---------|-----------|---------------|--------|
| EX-001 | supabase.auth.getUser() | múltiplos services | Identidade — não é query de tabela | Apenas user.id / user.email |
| EX-002 | supabase.auth.getSession() | customerService, financialService | Token para Edge Function | Apenas access_token |
| EX-003 | supabase.functions.invoke() | customerService, serviceOrderDetailService, vehicleService | Edge Functions — queryService não cobre | Apenas EFs conhecidas e documentadas |
| EX-004 | supabase.channel() / removeChannel() | serviceOrderDetailService | Realtime nativo — queryService não cobre | Apenas subscription de tabelas |
| EX-005 | supabase.rpc('import_quotation_prices') | quotationService | RPC sem tipo gerado (migration recente) | Apenas esta RPC. Remover quando tipos regenerados |
| EX-006 | fetch() com token | customerService, financialService | EFs que exigem Bearer explícito | create_customer_crm, admin_update_password, create_recurring_transactions |
| EX-007 | supabase em portais públicos | SupplierPortal, CustomerApprovalPortal | Sem sessão Supabase Auth | Apenas fetch() a EFs públicas. Sem .from() |
| EX-008 | resetSupabaseClient() | Login.tsx | Troca de strategy auth persistence | Apenas esta função. Sem query direta |
| EX-009 | supabase.storage | diagnosticUploadService, evidenceService (a confirmar) | Storage nativo — queryService não cobre | A documentar após auditoria ONDA 3 |

---

## 6. ONDAS — PLANO DE EXECUÇÃO

### ✅ ONDA 1 — CONCLUÍDA (SHA: 599bff4, 2026-03-17)

| Arquivo | Calls migradas | Exceções |
|---------|:--------------:|---------|
| serviceOrderService.ts | 20+ | — |
| serviceOrderDetailService.ts | 30+ | EX-001, EX-003, EX-004 |
| customerService.ts | 12+ | EX-001, EX-002, EX-003, EX-006 |
| vehicleService.ts | 11+ | EX-003 |

### ✅ ONDA 2 — CONCLUÍDA (SHA: 7efaf5e, 2026-03-17)

| Arquivo | Resultado | Detalhe |
|---------|:---------:|---------|
| appointmentService.ts | MIGRADO | 7 calls migradas |
| financialService.ts | JÁ CORRETO | EX-002 e EX-006 documentadas |
| laborCatalogService.ts | JÁ CORRETO | queryService + logger ok |
| partService.ts | JÁ CORRETO | queryService |
| purchaseOrderService.ts | JÁ CORRETO | queryService |
| quotationService.ts | JÁ CORRETO | EX-005 (RPC sem tipo) |

### ▶️ ONDA 3 — PRÓXIMA (5 services diagnóstico/tow)

| # | Arquivo | Suspeita |
|---|---------|---------|
| 11 | diagnosticService.ts | supabase direto, console.* |
| 12 | diagnosticUploadService.ts | supabase direto + storage legítimo |
| 13 | hypothesisService.ts | supabase direto |
| 14 | evidenceService.ts | supabase direto + storage legítimo |
| 15 | towService.ts | supabase direto |

### ONDA 4 — 14 services de suporte

knowledgeEngineService, scannerContextRecommendationService, notificationsService, auditService, healthService, confirmedRepairService, serviceIntentsService, crossOsPatternsService, biService, youtubeService, supplierService, symptomService, odometerCorrectionsService, diagnosticAnalyticsService

### ONDA 5 — 50+ console.* em pages web

Alvo principal: ServiceOrders, Customers, Vehicles, Appointments, Financial, Settings, Dashboard, Analytics

### ONDA 6 — Limpeza técnica

- Edge Function test-pdf em produção (remover)
- 2 migrations com timestamp 20260310000000 (verificar)
- 3 Dependabot alerts (1 critical, 1 high, 1 moderate) — INVESTIGAR
- README.md repo privado (verificar desatualização)

### ONDA 7 — Frentes de produto

- OBD Knowledge Base + Profiles + Rules + Painel (REQUER Seção 16 GEMINI.md antes de qualquer implementação)
- OBD Center R4/R5 (mobile)

---

## 7. HISTÓRICO DE RODADAS

| Rodada | SHA | Data | O que foi feito | tsc |
|--------|-----|------|-----------------|:---:|
| 3 — STATUS aprimorado | 37802fe | 2026-03-17 | STATUS reestruturado como memória operacional viva | ✅ |
| 3 — ONDA 2 | 7efaf5e | 2026-03-17 | appointmentService migrado + 5 auditados | ✅ |
| 2 — ONDA 1 | 599bff4 | 2026-03-17 | 4 services críticos migrados (63+ calls) | ✅ |
| 1 — Barramento | 3507fc1 | 2026-03-17 | Repo público + Seção 17 GEMINI.md | ✅ |
| — | dd67fd0 | 2026-03-17 | Hardening A: 5 services (whatsappMessages, tripScanner, telemetryAi, timeline, analytics) | ✅ |
| — | 2f5967f | 2026-03-17 | WhatsApp Inbox: 4 drifts corrigidos (last_timestamp, last_message, message_type, render img) | ✅ |
| — | b7ef9f8 | 2026-03-17 | FASE 4: Signed URLs bucket privado os-attachments | ✅ |
| — | bb99bfc | 2026-03-17 | FASE 3: Botões OBD restaurados + companyId corrigido | ✅ |
| — | f794224 | 2026-03-17 | FASE 0+1+2: Evidências Diagnósticas Inteligentes | ✅ |

---

## 8. DECISÕES FORMAIS REGISTRADAS

| ID | Decisão | Data |
|----|---------|------|
| D-001 | Catálogo MO, Peças, Cotações, Financeiro são web-only. Mobile não implementa. | 2026-03-17 |
| D-002 | queryService é o único ponto de acesso a tabelas. supabase.from() banido em UI. | 2026-03-17 |
| D-003 | logger é o único ponto de log. console.* banido em services. | 2026-03-17 |
| D-004 | Modo Evolução de Produto ativo desde 2026-03-16. | 2026-03-16 |
| D-005 | Multi-role implementado. ROLE_PERMISSION_MATRIX é a fonte canônica. | 2026-03-17 |
| D-006 | WhatsApp usa short OS codes para aprovação inbound. Hardening completo. | 2026-03-13 |
| D-007 | Repo público YouAutoCar-Status é o canal oficial de comunicação entre agentes. | 2026-03-17 |

---

## 9. RISCOS RESIDUAIS ABERTOS

| ID | Risco | Gravidade | Onda |
|----|-------|:---------:|:----:|
| R-001 | supabase.rpc('import_quotation_prices') sem tipo gerado — drift silencioso possível | MÉDIO | ONDA 4 |
| R-002 | 3 Dependabot alerts (1 critical, 1 high, 1 moderate) não investigados | ALTO | ONDA 6 |
| R-003 | Edge Function test-pdf em produção sem uso | MÉDIO | ONDA 6 |
| R-004 | 2 migrations com timestamp 20260310000000 — possível conflito | MÉDIO | ONDA 6 |
| R-005 | 50+ console.* em pages web — observabilidade degradada | BAIXO | ONDA 5 |
| R-006 | diagnosticUploadService e evidenceService podem ter supabase.storage — a auditar | BAIXO | ONDA 3 |
| R-007 | OBD Knowledge Base sem auditoria Seção 16 GEMINI.md | MÉDIO | ONDA 7 |

---

## 10. PROMPT SUGERIDO PARA PRÓXIMA SESSÃO

Copie o bloco abaixo integralmente e envie ao Antigravity:

---

Antigravity, continuando a Missão Mestra.

Leia o STATUS.md em:
https://raw.githubusercontent.com/You-Telecom-Provedor-de-internet/YouAutoCar-Status/main/STATUS.md

Estado atual: ONDA 2 concluída. SHA base: 7efaf5e. Rodada 3 em andamento.

Próxima execução: ONDA 3
Auditar e migrar supabase direto para queryService.from() nos 5 services de domínio diagnóstico:
- diagnosticService.ts
- diagnosticUploadService.ts
- hypothesisService.ts
- evidenceService.ts
- towService.ts

ATENÇÃO: diagnosticUploadService e evidenceService provavelmente usam supabase.storage — isso é EXCEÇÃO LEGÍTIMA (EX-009 no STATUS). Não remover storage. Documentar escopo exato.

Protocolo obrigatório:
1. Ler cada service completo antes de alterar (Seção 16 GEMINI.md)
2. Listar todos os pontos com supabase.from() e console.*
3. Migrar para queryService.from() e logger estruturado
4. Documentar exceções reais (auth, realtime, functions.invoke, storage)
5. Validar tsc --noEmit — 0 erros antes de commitar
6. Commit: refactor(services): ONDA 3 — [descricao]
7. Atualizar docs/audit/STATUS.md no repo privado
8. Atualizar STATUS.md no repo publico:
   https://raw.githubusercontent.com/You-Telecom-Provedor-de-internet/YouAutoCar-Status/main/STATUS.md

Formato de entrega:
BLOCO 1 — inventario por service (antes de alterar)
BLOCO 2 — arquivos alterados
BLOCO 3 — validacao (tsc = 0 erros)
BLOCO 4 — risco residual
BLOCO 5 — commit SHA
