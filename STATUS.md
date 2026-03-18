# YouAutoCar — MEMÓRIA OPERACIONAL DO PROJETO

## 2. ESTADO ATUAL

| Campo | Valor |
|-------|-------|
| **Rodada** | 17 |
| **SHA código** | `2f1c8fa` |
| **Data** | 2026-03-18 |
| **Modo** | EVOLUÇÃO DE PRODUTO — Limpeza de Legado Concluída |
| **tsc** | ✅ 0 erros |
| **build** | ✅ exit 0 |
| **flutter analyze** | ✅ 0 erros |
| **ONDA ativa** | Limpeza de Legado Concluída — Aguardando decisão próxima frente |

### Resumo da última rodada

**Rodada 17 — Limpeza de Legado Documental e Técnico:**

- ✅ `docs/AI/` 8 arquivos → arquivados em `_archive/` + README explicativo (D-014)
- ✅ `00_START_HERE.md` → refs a `docs/AI/` removidas, hierarquia corrigida
- ✅ `global_rules.md` §10.1 → fontes canônicas atualizadas (D-015)
- ✅ `dtcAiService.ts` → fallback OpenAI removido (-55 linhas)
- ✅ EF `dtc_analyze` → banner DEPRECATED (zero consumidores)

## 7. HISTÓRICO DE RODADAS

| Rodada | SHA | Data | O que foi feito | build |
|--------|-----|------|-----------------|:---:|
| 17 — Limpeza Legado | `2f1c8fa` | 2026-03-18 | 8 docs/AI → _archive. Fallback OpenAI removido. global_rules §10.1 corrigido. dtc_analyze DEPRECATED. | ✅ |
| 16 — Limpeza Owner | `43bd6b6` | 2026-03-18 | Dependabot 3 alerts fix (jspdf+esbuild). test-pdf deletado. Migration renomeada. | ✅ |
| 15 — ONDA 7F5 R4/R5 | `3516046` | 2026-03-18 | OBD Center R4/R5: Knowledge insights + KnowledgeInsightsCard. | ✅ |

## 8. DECISÕES FORMAIS

| ID | Decisão |
|----|--------|
| D-012 | `analisar-dtc` é canônica. `dtc_analyze` DEPRECATED com zero consumidores. |
| D-014 | `docs/AI/` inteira arquivada em `_archive/`. 8 handoffs/snapshots congelados. |
| D-015 | `global_rules.md` §10.1 corrigido. Fontes canônicas: GEMINI.md, STATUS.md, 10_AI_STRATEGY. |

## 9. RISCOS RESIDUAIS

| ID | Risco | Gravidade | Status |
|----|-------|:---------:|--------|
| R-001 | `import_quotation_prices` RPC sem tipo gerado | MÉDIO | EX-005 aceito |
| R-012 | EF `dtc_analyze` still deployed (zero consumers) | BAIXO | Owner decide remoção do deploy |

## 10. PROMPT SUGERIDO

```
Antigravity, Rodada 17 concluída. Limpeza de legado finalizada.

Resolvidos:
- docs/AI/ arquivado (8 files)
- Fallback OpenAI removido do web (-55 linhas)
- global_rules.md corrigido (fontes canônicas)
- dtc_analyze DEPRECATED (zero consumidores)

Decisões Owner:
1. CRUD knowledge_rules admin web
2. Remoção do deploy de dtc_analyze no Supabase
3. Nova frente de evolução — qual domínio priorizar?
```

---

> NOTA: Resumo compacto. Detalhes completos no repo privado.
