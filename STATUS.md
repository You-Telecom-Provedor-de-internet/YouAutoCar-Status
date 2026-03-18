# YouAutoCar — STATUS DE FECHAMENTO DO PRODUTO
<!-- ARQUIVO GERENCIADO AUTOMATICAMENTE PELO AGENTE ANTIGRAVITY -->
<!-- Atualizado a cada rodada de execução. Leia este arquivo para entender o estado atual. -->
<!-- URL pública: https://raw.githubusercontent.com/You-Telecom-Provedor-de-internet/YouAutoCar-Status/main/STATUS.md -->

---

## 🤖 PROTOCOLO MULTI-AGENTE

Este arquivo é o **barramento de memória compartilhada** entre os agentes do projeto.

| Agente | Papel | Acesso |
|--------|-------|--------|
| **Antigravity** | Executa, commita, atualiza este arquivo | Leitura + Escrita |
| **ChatGPT** | Lê, ajuda com prompts, sugere próximos passos | Leitura (raw URL) |
| **Owner** | Ponte operacional, decisão final | Leitura + Decisão |

**Como o ChatGPT usa este arquivo:**
Leia a seção `PRÓXIMA ONDA` e `PROBLEMAS CRÍTICOS ABERTOS` para sugerir prompts ao Antigravity.
Nunca sugira algo fora do que está no plano — respeite o GEMINI.md como lei.

---

## 📊 ESTADO ATUAL — RODADA 2

**SHA:** `4eaf8da`
**Data:** 2026-03-17
**Modo:** MISSÃO MESTRA — Fechamento Total (Zero Dívida Técnica Oculta)
**Repo privado:** `You-Telecom-Provedor-de-internet/YouAutoCarvAPP2`

---

## 🟢 DOMÍNIOS SÓLIDOS (Produto funcionando corretamente)

| Domínio | Web | Mobile | Observação |
|---------|:---:|:------:|-----------|
| Login / Auth / Claims | 🟢 | 🟢 | RLS + JWT Claims corretos |
| WhatsApp Inbox + Integração | 🟢 | N/A | Hardening completo. 4 drifts de campo corrigidos |
| OBD Evidence + Upload | 🟢 | 🟢 | FASE 0-4 completas — signed URLs, bucket privado |
| OBD Hipóteses + DTCs | 🟢 | 🟢 | Pipeline de IA restaurado |
| Catálogo de MO | 🟢 | 🔴 | Web-only por decisão formal |
| Catálogo de Peças | 🟢 | 🔴 | Web-only por decisão formal |
| Cotações/Fornecedores | 🟢 | 🔴 | Web-only por decisão formal |
| RLS / Banco | 🟢 | — | 167 migrations, multi-role implementado |
| CI/CD | 🟢 | 🟢 | ci.yml + watchdog.yml verdes |
| Edge Functions | 🟢 | — | 34/35 fechadas |
| **Ordens de Serviço (service)** | 🟢 | 🟡 | **ONDA 1 ✅ — queryService migrado** |
| **Clientes (service)** | 🟢 | 🟡 | **ONDA 1 ✅ — queryService migrado** |
| **Veículos (service)** | 🟢 | 🟢 | **ONDA 1 ✅ — queryService migrado** |

---

## 🟡 DOMÍNIOS PARCIAIS (Funcionam mas com dívida técnica)

| Domínio | Web | Mobile | Lacuna |
|---------|:---:|:------:|--------|
| Financeiro | 🟡 | 🔴 | `financialService` com `supabase` direto; mobile STUB |
| Agendamentos | 🟡 | 🟡 | `appointmentService` com `supabase` direto |
| OBD Center R4/R5 | 🟢 | 🟡 | Mobile: OBD Knowledge Base pendente |
| Guincho/Tow | 🟡 | 🟡 | `towService` com `supabase` direto |
| Dashboard/Analytics | 🟡 | 🟡 | console.* em pages |
| Configurações | 🟡 | 🟡 | Settings.tsx com 5 console.error |
| Logging/Observabilidade | 🟡 | 🟢 | 50+ console.* em pages web |

---

## 🔴 PROBLEMAS CRÍTICOS ABERTOS (Atacar em ordem)

### ✅ ONDA 1 — CONCLUÍDA (SHA: `599bff4` — 2026-03-17)

| # | Arquivo | Resultado |
|---|---------|---------|
| 1 | `serviceOrderService.ts` | ✅ queryService — 20+ calls migradas |
| 2 | `serviceOrderDetailService.ts` | ✅ queryService — 30+ calls migradas. Exceções: auth/functions/realtime documentadas |
| 3 | `customerService.ts` | ✅ queryService — 12+ calls migradas. Exceções: auth/functions documentadas |
| 4 | `vehicleService.ts` | ✅ queryService — 11+ calls migradas. Exceção: plate_lookup (Edge Function) |

**Exceções arquiteturais registradas na ONDA 1:**
- `supabase.auth.getUser()` / `auth.getSession()` — resolve identidade/token (não substitui com queryService)
- `supabase.functions.invoke()` — invoca Edge Functions (não é query de dados)
- `supabase.channel()` / `removeChannel()` — Realtime nativo (fora do escopo de queryService)

### ▶️ ONDA 2 — PRÓXIMA EXECUÇÃO

| # | Arquivo | Problema |
|---|---------|---------| 
| 5 | `financialService.ts` | `supabase` direto |
| 6 | `appointmentService.ts` | `supabase` direto |
| 7 | `laborCatalogService.ts` | `supabase` direto |
| 8 | `partService.ts` | `supabase` direto |
| 9 | `purchaseOrderService.ts` | `supabase` direto |
| 10 | `quotationService.ts` | `supabase` direto |

### ONDA 3 — Services de domínio específico

| # | Arquivo | Problema |
|---|---------|---------| 
| 11 | `diagnosticService.ts` | `supabase` direto |
| 12 | `diagnosticUploadService.ts` | `supabase` direto |
| 13 | `hypothesisService.ts` | `supabase` direto |
| 14 | `evidenceService.ts` | `supabase` direto |
| 15 | `towService.ts` | `supabase` direto |

### ONDA 4 — Services de suporte (14 services restantes)

`knowledgeEngineService`, `scannerContextRecommendationService`,
`notificationsService`, `auditService`, `healthService`,
`confirmedRepairService`, `serviceIntentsService`, `crossOsPatternsService`,
`biService`, `youtubeService`, `supplierService`, `symptomService`,
`odometerCorrectionsService`, `diagnosticAnalyticsService`

### ONDA 5 — Observabilidade em pages
- 50+ `console.*` em 15+ pages web → padronizar

### ONDA 6 — Limpeza
- `test-pdf` edge function em produção (remover)
- 2 migrations com timestamp `20260310000000` (verificar conflito)
- README.md do repo privado (verificar desatualização)

### ONDA 7 — Frentes de produto
- OBD Knowledge Base + Profiles + Rules + Painel (requer Seção 16 GEMINI.md)
- OBD Center R4/R5 (mobile)

---

## ✅ CONCLUÍDO

| O quê | SHA (repo privado) | Data |
|-------|-------------------|------|
| **ONDA 1: 4 services críticos migrados** | `599bff4` | 2026-03-17 |
| Setup repo público YouAutoCar-Status + duplo push | `26c798e` | 2026-03-17 |
| Barramento multi-agente: STATUS.md público + Seção 17 GEMINI.md | `3507fc1` | 2026-03-17 |
| Hardening A: 5 services migrados | `dd67fd0` | 2026-03-17 |
| WhatsApp Inbox: 4 drifts de campo corrigidos | `2f5967f` | 2026-03-17 |
| FASE 4: Signed URLs bucket privado | `b7ef9f8` | 2026-03-17 |
| FASE 3: OBD Restore | `bb99bfc` | 2026-03-17 |
| FASE 0+1+2: Evidências Diagnósticas | `f794224` | 2026-03-17 |

---

## 💬 PROMPT SUGERIDO PARA PRÓXIMA SESSÃO

IMPORTANTE: Copie o bloco abaixo integralmente e envie ao Antigravity.

---

Antigravity, continuando a Missão Mestra.

Leia o STATUS.md público em:
https://raw.githubusercontent.com/You-Telecom-Provedor-de-internet/YouAutoCar-Status/main/STATUS.md

Estado atual: ONDA 1 concluída. SHA base: 599bff4.

Próxima execução: ONDA 2
Migrar supabase direto para queryService.from() nos 6 services secundários:
- financialService.ts
- appointmentService.ts
- laborCatalogService.ts
- partService.ts
- purchaseOrderService.ts
- quotationService.ts

Protocolo obrigatório (igual à ONDA 1):
1. Ler cada service completo antes de alterar
2. Listar todos os pontos com supabase.from() e console.*
3. Migrar para queryService.from() e logger estruturado
4. Documentar exceções reais (auth, realtime, functions.invoke)
5. Validar tsc --noEmit — 0 erros
6. Commit com SHA
7. Atualizar docs/audit/STATUS.md no repo privado E STATUS.md no repo público:
   https://raw.githubusercontent.com/You-Telecom-Provedor-de-internet/YouAutoCar-Status/main/STATUS.md

---
