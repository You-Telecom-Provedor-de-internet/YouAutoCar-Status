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

## 📊 ESTADO ATUAL — RODADA 1

**SHA base:** `3507fc1`
**Data:** 2026-03-17
**Modo:** MISSÃO MESTRA — Fechamento Total (Zero Dívida Técnica Oculta)

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

---

## 🟡 DOMÍNIOS PARCIAIS (Funcionam mas com dívida técnica)

| Domínio | Web | Mobile | Lacuna |
|---------|:---:|:------:|--------|
| Ordens de Serviço | 🟡 | 🟡 | `serviceOrderService` + `serviceOrderDetailService` com `supabase` direto |
| Clientes | 🟡 | 🟡 | `customerService` com `supabase` direto |
| Veículos | 🟡 | 🟢 | `vehicleService` com `supabase` direto |
| Financeiro | 🟡 | 🔴 | `financialService` com `supabase` direto; mobile STUB |
| Agendamentos | 🟡 | 🟡 | `appointmentService` com `supabase` direto |
| OBD Center R4/R5 | 🟢 | 🟡 | Mobile: OBD Knowledge Base pendente |
| Guincho/Tow | 🟡 | 🟡 | `towService` com `supabase` direto |
| Dashboard/Analytics | 🟡 | 🟡 | console.* em pages |
| Configurações | 🟡 | 🟡 | Settings.tsx com 5 console.error |
| Logging/Observabilidade | 🟡 | 🟢 | 50+ console.* em pages web |

---

## 🔴 PROBLEMAS CRÍTICOS ABERTOS (Atacar em ordem)

### ▶️ ONDA 1 — Services críticos (PRÓXIMA EXECUÇÃO)

| # | Arquivo | Problema | Impacto |
|---|---------|---------|---------|
| 1 | `apps/web/src/services/serviceOrderService.ts` | `supabase` direto — core do produto | CRÍTICO |
| 2 | `apps/web/src/services/serviceOrderDetailService.ts` | `supabase` direto — tabs da OS | CRÍTICO |
| 3 | `apps/web/src/services/customerService.ts` | `supabase` direto — base de clientes | CRÍTICO |
| 4 | `apps/web/src/services/vehicleService.ts` | `supabase` direto — entidade central | CRÍTICO |

### ONDA 2 — Services secundários (após Onda 1)

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
| Barramento multi-agente: STATUS.md público + Seção 17 GEMINI.md | `3507fc1` | 2026-03-17 |
| Hardening A: 5 services migrados (supabase→queryService + console→logger) | `dd67fd0` | 2026-03-17 |
| WhatsApp Inbox: 4 drifts de campo corrigidos | `2f5967f` | 2026-03-17 |
| FASE 4: Signed URLs para bucket privado os-attachments | `b7ef9f8` | 2026-03-17 |
| FASE 3: Botões OBD restaurados + companyId corrigido | `bb99bfc` | 2026-03-17 |
| FASE 0+1+2: Evidências Diagnósticas Inteligentes | `f794224` | 2026-03-17 |

---

## 📋 HISTÓRICO DE RODADAS

| Rodada | SHA | Data | Resultado |
|--------|-----|------|-----------|
| Setup barramento multi-agente | `3507fc1` | 2026-03-17 | ✅ Infra de colaboração ativa |
| Auditoria Global R1 | `dd67fd0` | 2026-03-17 | Matriz 20 domínios, 7 ondas |
| Hardening A (5 services) | `dd67fd0` | 2026-03-17 | ✅ Concluído |
| WhatsApp Inbox Fix | `2f5967f` | 2026-03-17 | ✅ Concluído |
| FASE 4 – Signed URLs | `b7ef9f8` | 2026-03-17 | ✅ Concluído |
| FASE 3 – OBD Restore | `bb99bfc` | 2026-03-17 | ✅ Concluído |
| FASE 0+1+2 – Evidências | `f794224` | 2026-03-17 | ✅ Concluído |

---

## 🧠 REGRAS DE GOVERNANÇA ATIVAS

Regras completas: `GEMINI.md` no repo privado `YouAutoCarvAPP2`.

**Resumo das regras críticas:**
1. Toda implementação começa com auditoria (Seção 16)
2. Nenhuma page/hook/component acessa Supabase diretamente
3. Todo acesso a dados passa por services com `queryService.from()`
4. `console.*` substituídos por `logger` nos services
5. Exceções arquiteturais documentadas na Seção 6 do GEMINI.md
6. `tsc --noEmit`: 0 erros obrigatório antes de qualquer commit
7. STATUS.md atualizado a cada rodada (Seção 17 do GEMINI.md)

---

## 💬 PROMPT SUGERIDO PARA PRÓXIMA SESSÃO

> Antigravity, continuando a Missão Mestra.
> Leia o STATUS.md em `docs/audit/STATUS.md` do repo principal.
> Estado atual: Rodada 1 concluída. SHA base: `3507fc1`.
>
> **Próxima execução: ONDA 1**
> Migrar `supabase` direto → `queryService.from()` nos 4 services críticos:
> - `serviceOrderService.ts`
> - `serviceOrderDetailService.ts`
> - `customerService.ts`
> - `vehicleService.ts`
>
> Protocolo obrigatório:
> 1. Ler cada service completo
> 2. Identificar todos os pontos com `supabase.from()` e `console.*`
> 3. Migrar para `queryService.from()` e `logger`
> 4. Documentar exceções se houver (auth, storage, realtime)
> 5. Validar `tsc --noEmit` — 0 erros
> 6. Commit com SHA
> 7. Atualizar `docs/audit/STATUS.md` no repo privado E `STATUS.md` neste repo público
