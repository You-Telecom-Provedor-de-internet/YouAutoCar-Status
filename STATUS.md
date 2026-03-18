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

## 2. ESTADO ATUAL

| Campo | Valor |
|-------|-------|
| **Rodada** | 16 |
| **SHA código** | `43bd6b6` |
| **SHA status** | `43bd6b6` |
| **Data** | 2026-03-18 |
| **Modo** | EVOLUÇÃO DE PRODUTO — Limpeza Owner Concluída |
| **tsc** | ✅ 0 erros |
| **build** | ✅ exit 0 |
| **flutter analyze** | ✅ 0 erros |
| **ONDA ativa** | Limpeza Owner Concluída — Aguardando decisão próxima frente |
| **Próxima ação obrigatória** | Owner decide: CRUD admin web knowledge_rules / nova frente |

### Resumo da última rodada

**Rodada 16 — Limpeza Pendente do Owner (R-002, R-003, R-004):**

- ✅ Dependabot: 3 alerts resolvidos (jspdf Critical+High → override >=4.2.1, esbuild Moderate → vitest ^4.1.0)
- ✅ `test-pdf`: Edge Function removida (3 arquivos, zero consumidores)
- ✅ Migrations: `20260310000000_auto_dispatch_engine.sql` renomeada para `20260310000001`
- ✅ npm audit: 0 vulnerabilities em apps/web e domain-service-orders

---

## 7. HISTÓRICO DE RODADAS

| Rodada | SHA | Data | O que foi feito | build |
|--------|-----|------|-----------------|:---:|
| 16 — Limpeza Owner | `43bd6b6` | 2026-03-18 | Dependabot 3 alerts fix (jspdf+esbuild). test-pdf deletado. Migration renomeada. | ✅ |
| 15 — ONDA 7F5 R4/R5 | `3516046` | 2026-03-18 | OBD Center R4/R5: Knowledge insights + KnowledgeInsightsCard. Zero estruturas novas. | ✅ |
| 14 — ONDA 7F4 | `23ad8c3` | 2026-03-18 | updated_at + trigger. Tipagem web corrigida. D-013 catálogo global. | ✅ |

## 9. RISCOS RESIDUAIS ABERTOS

| ID | Risco | Gravidade | Status | Onda |
|----|-------|:---------:|--------|:----:|
| R-001 | `supabase.rpc('import_quotation_prices')` sem tipo gerado | MÉDIO | EX-005 aceito | Após regen de tipos |
| R-002 | ~~3 Dependabot alerts~~ | ~~ALTO~~ | ✅ RESOLVIDO — Rodada 16 | — |
| R-003 | ~~Edge Function `test-pdf`~~ | ~~MÉDIO~~ | ✅ RESOLVIDO — Rodada 16 | — |
| R-004 | ~~2 migrations duplicadas~~ | ~~MÉDIO~~ | ✅ RESOLVIDO — Rodada 16 | — |

## 10. PROMPT SUGERIDO PARA PRÓXIMA SESSÃO

```
Antigravity, Rodada 16 concluída. Limpeza Owner finalizada.

Resolvidos: Dependabot (3 alerts), test-pdf (removida), migrations duplicadas (renomeada).
OBD Center R1-R5 completo. ONDA 7 Fases 1-5 concluídas.

Decisões Owner necessárias para prosseguir:
1. CRUD knowledge_rules admin web — construir tela de gestão?
2. Fase 3C — avaliar remoção do fallback dtc_analyze no web
3. Nova frente de evolução — qual domínio priorizar?
```

### Itens Owner (paralelos):
1. **CRUD knowledge_rules admin** — decisão de escopo
2. **Fase 3C futura** — avaliar remoção do fallback `dtc_analyze` no web
3. **Nova frente** — qual domínio priorizar?

---

> NOTA: Este é um resumo compacto do STATUS.md completo que está no repo privado.
> Para detalhes completos de ondas, exceções, decisões e contratos, consulte o repo privado.
