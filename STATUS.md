# YouAutoCar — MEMÓRIA OPERACIONAL DO PROJETO
<!-- ARQUIVO GERENCIADO AUTOMATICAMENTE PELO AGENTE ANTIGRAVITY -->
<!-- Este arquivo é a fonte de verdade operacional entre Antigravity, ChatGPT e Owner -->
<!-- URL PÚBLICA (ChatGPT lê aqui sem autenticação): -->
<!-- https://raw.githubusercontent.com/You-Telecom-Provedor-de-internet/YouAutoCar-Status/main/STATUS.md -->

---

## 🔖 ÍNDICE RÁPIDO

1. PROTOCOLO MULTI-AGENTE
2. REGRA DE USO DO BARRAMENTO
3. ESTADO ATUAL
4. MAPA DE DOMÍNIOS
5. ARQUITETURA — CONTRATOS OBRIGATÓRIOS
6. EXCEÇÕES ARQUITETURAIS VIGENTES
7. ONDAS — PLANO DE EXECUÇÃO
8. HISTÓRICO DE RODADAS
9. DECISÕES FORMAIS REGISTRADAS
10. RISCOS RESIDUAIS ABERTOS
11. PROMPT SUGERIDO PARA PRÓXIMA SESSÃO

---

## 1. PROTOCOLO MULTI-AGENTE

| Agente | Papel | O que pode fazer |
|--------|-------|-----------------|
| Antigravity | Execução + Commit | Lê e atualiza STATUS.md, executa código, faz push para ambos os repos |
| ChatGPT | Consultoria + Prompts | Lê STATUS.md via URL pública, sugere próximos prompts ao Owner |
| Owner | Ponte + Decisão | Repassa prompts entre agentes, decide prioridades e escopo |

URL para leitura pelo ChatGPT (raw, sem autenticação):
https://raw.githubusercontent.com/You-Telecom-Provedor-de-internet/YouAutoCar-Status/main/STATUS.md

---

## 🔁 REGRA DE USO DO BARRAMENTO

> Esta seção é parte obrigatória da governança. Leia antes de agir.

Este arquivo É a memória operacional do projeto.

O sistema funciona assim:

GEMINI.md     = lei permanente do projeto
STATUS.md     = memória viva da sessão atual
ChatGPT       = coordena usando a memória viva
Antigravity   = executa e atualiza a memória viva
Owner         = decide prioridades e fecha o ciclo

Protocolo obrigatório de abertura de sessão:

Quando o Owner abrir uma nova sessão com o ChatGPT:
1. Enviar ao ChatGPT: "Sincronize pelo STATUS em
   https://raw.githubusercontent.com/You-Telecom-Provedor-de-internet/YouAutoCar-Status/main/STATUS.md
   e sugira o próximo prompt."
2. ChatGPT lê → identifica ONDA ativa + PROMPT SUGERIDO
3. Owner envia o prompt gerado ao Antigravity
4. Antigravity executa → atualiza STATUS privado + público → entrega SHA
5. Ciclo fecha

O que o ChatGPT DEVE fazer ao ler este arquivo:
- Identificar a seção ESTADO ATUAL e a ONDA ativa
- Usar o bloco PROMPT SUGERIDO como base — não inventar escopo
- Alertar o Owner se detectar divergência entre STATUS e o que o Owner descreve
- Nunca sugerir ONDAs futuras se a ONDA ativa ainda estiver aberta

O que o Antigravity DEVE fazer ao encerrar cada rodada:
- Atualizar docs/audit/STATUS.md no repo privado YouAutoCarvAPP2
- Atualizar STATUS.md no repo público YouAutoCar-Status
- Registrar SHA do commit, domínios afetados, riscos e prompt sugerido
- Rodada sem dupla atualização = rodada INVÁLIDA (GEMINI.md Seção 18)

---

## 2. ESTADO ATUAL

| Campo | Valor |
|-------|-------|
| Rodada | 3 |
| SHA privado | a0e75b8 |
| Data | 2026-03-17 |
| Modo | MISSÃO MESTRA — Fechamento Total (Zero Dívida Técnica Oculta) |
| tsc --noEmit | 0 erros |
| ONDA ativa | ONDA 3 (5 services de diagnóstico) |
| Próxima ação | Executar ONDA 3 |

Resumo da última rodada:
ONDA 2 executada. 6 services auditados. 5 já corretos. appointmentService: 7 calls migradas.
GEMINI.md atualizado: Seção 18 formaliza STATUS.md como memória operacional permanente.
STATUS.md reestruturado com 10 seções, contratos, exceções numeradas, decisões formais e riscos.

---

## 3. MAPA DE DOMÍNIOS

### Web

| Domínio | Service | UI | Observação |
|---------|:---:|:---:|-----------|
| Login / Auth | 🟢 | 🟢 | RLS + JWT ok |
| Ordens de Serviço | 🟢 | 🟢 | ONDA 1 — migrado |
| Clientes | 🟢 | 🟢 | ONDA 1 — migrado |
| Veículos | 🟢 | 🟢 | ONDA 1 — migrado |
| Agendamentos | 🟢 | 🟡 | ONDA 2 — migrado. Pages: console.* residual |
| Financeiro | 🟢 | 🟡 | ONDA 2 — já correto. Pages: console.* residual |
| Catálogo MO | 🟢 | 🟢 | ONDA 2 — já correto |
| Peças | 🟢 | 🟡 | ONDA 2 — já correto. Pages: console.* residual |
| Pedidos de Compra | 🟢 | 🟡 | ONDA 2 — já correto |
| Cotações | 🟢 | 🟡 | ONDA 2 — já correto. EX-005 (RPC sem tipo) |
| WhatsApp | 🟢 | 🟢 | Hardening completo |
| OBD / DTCs / Evidence | 🟢 | 🟢 | Pipeline IA ok. FASE 0-4 ok |
| Diagnóstico (service) | 🟡 | 🟡 | ONDA 3 pendente |
| Diagnóstico Upload | 🟡 | 🟡 | ONDA 3 pendente — storage legítimo possível |
| Hipóteses | 🟡 | 🟡 | ONDA 3 pendente |
| Evidências | 🟡 | 🟡 | ONDA 3 pendente — storage legítimo possível |
| Guincho/Tow | 🟡 | 🟡 | ONDA 3 pendente |
| Dashboard/Analytics | 🟡 | 🟡 | console.* em pages. biService ONDA 4 |
| Configurações | 🟡 | 🟡 | Settings.tsx: 5 console.error |
| Edge Functions | 🟢 | — | 34/35 fechadas. test-pdf em produção (ONDA 6) |
| RLS / Banco | 🟢 | — | 167 migrations. Multi-role. ROLE_PERMISSION_MATRIX |
| CI/CD | 🟢 | — | ci.yml + watchdog.yml verdes |

### Mobile

| Domínio | Status | Observação |
|---------|:---:|-----------|
| Login / Auth | 🟢 | Riverpod ok |
| OBD ELM327 | 🟢 | Bluetooth RFCOMM ok |
| OBD Center (UI) | 🟡 | R4/R5 pendentes |
| Viagens / Telemetria | 🟢 | ok |
| Alertas OBD | 🟢 | ok |
| Catálogo MO | 🔴 | Web-only — decisão D-001 |
| Peças | 🔴 | Web-only — decisão D-001 |
| Cotações | 🔴 | Web-only — decisão D-001 |
| Financeiro | 🔴 | Web-only — decisão D-001 |

---

## 4. ARQUITETURA — CONTRATOS OBRIGATÓRIOS

Acesso a dados:
- pages/components/hooks → supabase.from() NUNCA
- services → queryService.from() SEMPRE
- Exceções aceitas APENAS em services (ver Seção 6): supabase.auth.*, supabase.functions.*, supabase.channel(), supabase.storage, fetch() com token

Logging:
- console.log → logger.info
- console.error → logger.error
- console.warn → logger.warn

Identidade:
- auth.uid() direto PROIBIDO → usar get_user_id()

Roles:
- role === 'admin' inline PROIBIDO → usar hasPermission() ou ROLE_PERMISSION_MATRIX

---

## 5. EXCEÇÕES ARQUITETURAIS VIGENTES

| ID | Exceção | Arquivo(s) | Justificativa | Escopo |
|----|---------|-----------|---------------|--------|
| EX-001 | supabase.auth.getUser() | múltiplos services | Identidade — não é query de tabela | Apenas user.id / user.email |
| EX-002 | supabase.auth.getSession() | customerService, financialService | Token para Edge Function | Apenas access_token |
| EX-003 | supabase.functions.invoke() | customerService, serviceOrderDetailService, vehicleService | Edge Functions — queryService não cobre | Apenas EFs documentadas |
| EX-004 | supabase.channel() / removeChannel() | serviceOrderDetailService | Realtime nativo | Apenas subscription |
| EX-005 | supabase.rpc('import_quotation_prices') | quotationService | RPC sem tipo gerado — migration recente | Apenas esta RPC. Remover quando tipos regenerados |
| EX-006 | fetch() com token | customerService, financialService | EFs com Bearer explícito | create_customer_crm, admin_update_password, create_recurring_transactions |
| EX-007 | supabase em portais públicos | SupplierPortal, CustomerApprovalPortal | Sem sessão Supabase Auth | Apenas fetch() a EFs. Sem .from() |
| EX-008 | resetSupabaseClient() | Login.tsx | Troca de strategy auth persistence | Apenas esta função. Sem query direta |
| EX-009 | supabase.storage | diagnosticUploadService, evidenceService (a confirmar) | Storage nativo | A documentar após ONDA 3 |

---

## 6. ONDAS — PLANO DE EXECUÇÃO

### ✅ ONDA 1 — CONCLUÍDA (SHA: 599bff4, 2026-03-17, tsc: 0 erros)

serviceOrderService (20+), serviceOrderDetailService (30+), customerService (12+), vehicleService (11+)

### ✅ ONDA 2 — CONCLUÍDA (SHA: 7efaf5e, 2026-03-17, tsc: 0 erros)

appointmentService (MIGRADO — 7 calls), financialService (JÁ CORRETO), laborCatalogService (JÁ CORRETO), partService (JÁ CORRETO), purchaseOrderService (JÁ CORRETO), quotationService (JÁ CORRETO — EX-005)

### ▶️ ONDA 3 — PRÓXIMA

| # | Arquivo | Suspeita |
|---|---------|---------|
| 11 | diagnosticService.ts | supabase direto, console.* |
| 12 | diagnosticUploadService.ts | supabase direto + storage legítimo |
| 13 | hypothesisService.ts | supabase direto |
| 14 | evidenceService.ts | supabase direto + storage legítimo |
| 15 | towService.ts | supabase direto |

### ONDA 4 — 14 services de suporte

knowledgeEngineService, scannerContextRecommendationService, notificationsService, auditService, healthService, confirmedRepairService, serviceIntentsService, crossOsPatternsService, biService, youtubeService, supplierService, symptomService, odometerCorrectionsService, diagnosticAnalyticsService

### ONDA 5 — console.* em pages web (50+ ocorrências, 15+ pages)

### ONDA 6 — Limpeza técnica

- test-pdf edge function em produção (remover)
- 2 migrations timestamp 20260310000000 (verificar)
- Dependabot alerts: 1 critical, 1 high, 1 moderate — INVESTIGAR
- README.md repo privado (verificar)

### ONDA 7 — Frentes de produto

- OBD Knowledge Base (REQUER Seção 16 GEMINI.md antes de tudo)
- OBD Center R4/R5 (mobile)

---

## 7. HISTÓRICO DE RODADAS

| Rodada | SHA | Data | O que foi feito | tsc |
|--------|-----|------|-----------------|:---:|
| 3 — GEMINI.md Seção 18 + STATUS barramento | a0e75b8 | 2026-03-17 | STATUS como memória permanente formalizada | ✅ |
| 3 — STATUS reestruturado | 42687ba | 2026-03-17 | 10 seções, contratos, exceções, decisões, riscos | ✅ |
| 3 — ONDA 2 | 7efaf5e | 2026-03-17 | appointmentService migrado + 5 auditados | ✅ |
| 2 — ONDA 1 | 599bff4 | 2026-03-17 | 4 services críticos migrados (63+ calls) | ✅ |
| 1 — Barramento | 3507fc1 | 2026-03-17 | Repo público + Seção 17 GEMINI.md | ✅ |
| — | dd67fd0 | 2026-03-17 | Hardening A: 5 services | ✅ |
| — | 2f5967f | 2026-03-17 | WhatsApp Inbox: 4 drifts corrigidos | ✅ |
| — | b7ef9f8 | 2026-03-17 | FASE 4: Signed URLs bucket privado | ✅ |
| — | bb99bfc | 2026-03-17 | FASE 3: OBD restored | ✅ |
| — | f794224 | 2026-03-17 | FASE 0+1+2: Evidências Diagnósticas | ✅ |

---

## 8. DECISÕES FORMAIS REGISTRADAS

| ID | Decisão | Data |
|----|---------|------|
| D-001 | Catálogo MO, Peças, Cotações, Financeiro são web-only | 2026-03-17 |
| D-002 | queryService é o único ponto de acesso a tabelas. supabase.from() banido em UI | 2026-03-17 |
| D-003 | logger é o único ponto de log. console.* banido em services | 2026-03-17 |
| D-004 | Modo Evolução de Produto ativo desde 2026-03-16 | 2026-03-16 |
| D-005 | Multi-role implementado. ROLE_PERMISSION_MATRIX é a fonte canônica | 2026-03-17 |
| D-006 | WhatsApp usa short OS codes para aprovação inbound. Hardening completo | 2026-03-13 |
| D-007 | Repo público YouAutoCar-Status é o canal oficial de comunicação entre agentes | 2026-03-17 |
| D-008 | STATUS.md é memória operacional permanente do projeto — lei de segunda ordem (GEMINI.md Seção 18) | 2026-03-17 |

---

## 9. RISCOS RESIDUAIS ABERTOS

| ID | Risco | Gravidade | Onda |
|----|-------|:---------:|:----:|
| R-001 | supabase.rpc('import_quotation_prices') sem tipo gerado | MÉDIO | ONDA 4 |
| R-002 | 3 Dependabot alerts (1 critical, 1 high, 1 moderate) | ALTO | ONDA 6 |
| R-003 | Edge Function test-pdf em produção sem uso | MÉDIO | ONDA 6 |
| R-004 | 2 migrations com timestamp 20260310000000 | MÉDIO | ONDA 6 |
| R-005 | 50+ console.* em pages web | BAIXO | ONDA 5 |
| R-006 | diagnosticUploadService / evidenceService — storage a auditar | BAIXO | ONDA 3 |
| R-007 | OBD Knowledge Base sem auditoria prévia (Seção 16) | MÉDIO | ONDA 7 |

---

## 10. PROMPT SUGERIDO PARA PRÓXIMA SESSÃO

Copie o bloco abaixo integralmente e envie ao Antigravity:

---

Antigravity, continuando a Missão Mestra.

Leia o STATUS.md em:
https://raw.githubusercontent.com/You-Telecom-Provedor-de-internet/YouAutoCar-Status/main/STATUS.md

Estado atual: ONDA 2 concluída. SHA base: a0e75b8. Rodada 3 ativa.

Próxima execução: ONDA 3
Auditar e migrar supabase direto para queryService.from() nos 5 services de domínio diagnóstico:
- diagnosticService.ts
- diagnosticUploadService.ts
- hypothesisService.ts
- evidenceService.ts
- towService.ts

ATENÇÃO: diagnosticUploadService e evidenceService provavelmente usam supabase.storage — EXCEÇÃO LEGÍTIMA (EX-009 no STATUS). Não remover storage. Documentar escopo exato.

Protocolo obrigatório:
1. Ler cada service completo antes de alterar (Seção 16 GEMINI.md)
2. Listar todos os pontos com supabase.from() e console.*
3. Migrar para queryService.from() e logger estruturado
4. Documentar exceções reais (auth, realtime, functions.invoke, storage)
5. Validar tsc --noEmit — 0 erros
6. Commit: refactor(services): ONDA 3 — [descricao]
7. Atualizar docs/audit/STATUS.md no repo privado
8. Atualizar STATUS.md no repo publico:
   https://raw.githubusercontent.com/You-Telecom-Provedor-de-internet/YouAutoCar-Status/main/STATUS.md

Formato de entrega:
BLOCO 1 — inventario por service
BLOCO 2 — arquivos alterados
BLOCO 3 — validacao (tsc = 0 erros)
BLOCO 4 — risco residual
BLOCO 5 — commit SHA
