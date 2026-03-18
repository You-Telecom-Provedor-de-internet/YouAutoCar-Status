# YouAutoCar — MEMÓRIA OPERACIONAL DO PROJETO
<!-- ARQUIVO GERENCIADO AUTOMATICAMENTE PELO AGENTE ANTIGRAVITY -->
<!-- Este arquivo é a fonte de verdade operacional entre Antigravity, ChatGPT e Owner -->
<!-- ⚠️ Não edite manualmente sem atualizar também o repo público -->
<!-- URL PÚBLICA (ChatGPT lê aqui): -->
<!-- https://raw.githubusercontent.com/You-Telecom-Provedor-de-internet/YouAutoCar-Status/main/STATUS.md -->
<!-- Repo público: https://github.com/You-Telecom-Provedor-de-internet/YouAutoCar-Status -->

---

## 2. ESTADO ATUAL

| Campo | Valor |
|-------|-------|
| **Rodada** | 13 |
| **SHA código** | `780b5e6` |
| **SHA status** | `0fe3a8b` |
| **Data** | 2026-03-18 |
| **Modo** | EVOLUÇÃO DE PRODUTO — ONDA 7 Fase 3 Concluída |
| **tsc** | ✅ 0 erros |
| **build** | ✅ exit 0 |
| **flutter analyze** | ✅ 0 erros |
| **ONDA ativa** | ONDA 7 Fase 4+ pendente (Owner) |
| **Próxima ação obrigatória** | Owner decide: ONDA 7 Fase 4 (knowledge_rules CRUD) / Fase 5 (OBD R4/R5) |

### Resumo da última rodada

**Rodada 13 — ONDA 7 Fase 3: Duplicação DTC Resolvida (D-012):**

**Classificação**: V1/V2 — `dtc_analyze` é legado (V1), `analisar-dtc` é a função canônica (V2).

| Critério | `analisar-dtc` (canônica) | `dtc_analyze` (legado) |
|----------|:-------------------------:|:----------------------:|
| IA primária | Gemini 2.0 Flash | OpenAI GPT-3.5-turbo |
| Fallback | OpenAI GPT-4o-mini + determinístico | Mock simples |
| Contexto OBD | ✅ engine_profile, freeze_frame, mode06 | ❌ |
| Persistência BD | ✅ Salva em `diagnosticos` | ❌ |
| Linhas | 327 | 162 |

**Consumidores após migração:**
- Web `dtcAiService.ts`: `analisar-dtc` (primária) → `dtc_analyze` (fallback) — **inalterado**
- Mobile `DtcAiService.dart` (OBD): `analisar-dtc` — **já correto**
- Mobile `diagnostics_repository_impl.dart` (VM): `analisar-dtc` — **MIGRADO nesta rodada** (era `dtc_analyze`)

**Arquivos alterados:**
- `diagnostics_repository_impl.dart` — invoke `dtc_analyze` → `analisar-dtc` + adaptador de formato
- `dtc_analyze/index.ts` — banner ⚠️ LEGADO adicionado

---

## D-012 — Decisão Formal

`analisar-dtc` é a função canônica para análise DTC. `dtc_analyze` é legado V1 (mantido apenas como fallback web). Mobile totalmente migrado. SHA: `780b5e6`.

---

## HISTÓRICO RECENTE

| Rodada | SHA | Data | O que foi feito | build |
|--------|-----|------|-----------------|:---:|
| 13 — ONDA 7F3 DTC | `780b5e6` | 2026-03-18 | D-012: analisar-dtc canônica, dtc_analyze legado. Mobile migrado. Banner legado. | ✅ |
| 12 — Validação ONDA 7 | `7192854` | 2026-03-18 | 5/5 RPCs testadas. 2/2 services OK. 2/2 dashboards HTTP 200. | ✅ |
| 11 — Limpeza Doc | `477af2a` | 2026-03-18 | 15 depreciados, 2 atualizados, 3 removidos. | ✅ |

---

## PRÓXIMO PROMPT SUGERIDO

```
Antigravity, ONDA 7 Fases 1-3 concluídas (Rodada 13).

Todas as 5 RPCs funcionais. Dashboards OK. Duplicação DTC resolvida (D-012).

Decisões Owner necessárias para prosseguir:
1. CRUD knowledge_rules — web-only ou web+mobile?
2. Multi-tenant knowledge_rules — regras por company ou globais?
3. OBD Center R4/R5 mobile — integração com Knowledge Base
```

### Itens Owner (paralelos):
1. **Dependabot alerts** — 1 critical, 1 high, 1 moderate
2. **Edge Function `test-pdf`** — remover?
3. **Migrations duplicadas** `20260310000000` — renomear?
4. **CRUD knowledge_rules** — decisão de escopo
5. **Fase 3C futura** — avaliar remoção do fallback `dtc_analyze` no web
