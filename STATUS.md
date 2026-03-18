# YouAutoCar — MEMÓRIA OPERACIONAL DO PROJETO
<!-- ARQUIVO GERENCIADO AUTOMATICAMENTE PELO AGENTE ANTIGRAVITY -->
<!-- Este arquivo é a fonte de verdade operacional entre Antigravity, ChatGPT e Owner -->

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

## D-013 — Decisão Formal

`knowledge_rules` é catálogo global — sem `company_id`. Regras técnicas universais. Se empresa precisar de regras custom, será tabela separada. SHA: `23ad8c3`.

---

## HISTÓRICO RECENTE

| Rodada | SHA | Data | O que foi feito | build |
|--------|-----|------|-----------------|:---:|
| 14 — ONDA 7F4 | `23ad8c3` | 2026-03-18 | updated_at + trigger. Tipagem web corrigida. D-013 catálogo global. | ✅ |
| 13 — ONDA 7F3 DTC | `780b5e6` | 2026-03-18 | D-012: analisar-dtc canônica, dtc_analyze legado. Mobile migrado. | ✅ |
| 12 — Validação ONDA 7 | `7192854` | 2026-03-18 | 5/5 RPCs testadas. 2/2 services OK. 2/2 dashboards HTTP 200. | ✅ |

---

## PRÓXIMO PROMPT SUGERIDO

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
