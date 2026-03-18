# YouAutoCar — Status da Missão Mestra

> 🤖 Barramento de memória compartilhada entre **Antigravity** (executa) e **ChatGPT** (sugere prompts).

Este repositório é **somente leitura** para agentes externos.
Todo o código do produto está no repositório privado `YouAutoCarvAPP2`.

---

## 📄 Arquivo principal

👉 **[STATUS.md](./STATUS.md)** — Estado real do produto, atualizado a cada rodada de execução.

**URL direta (raw):**
```
https://raw.githubusercontent.com/You-Telecom-Provedor-de-internet/YouAutoCar-Status/main/STATUS.md
```

---

## 🤖 Como usar (ChatGPT)

1. Acesse a URL raw acima
2. Leia a seção `PRÓXIMA ONDA` para entender o que vem a seguir
3. Leia `PROBLEMAS CRÍTICOS ABERTOS` para contexto completo
4. Use o `PROMPT SUGERIDO` como base para a próxima sessão com o Antigravity
5. Nunca sugira implementações fora do plano — o `GEMINI.md` no repo privado é lei

---

## ⚙️ Como funciona

```
Antigravity (executa + commita)
    ↓ atualiza STATUS.md aqui e no repo privado
    ↓ git push (ambos os repos)
GitHub públic (este repo)
    ↓ raw URL sempre atualizada
ChatGPT (lê + sugere prompts)
    → "O STATUS diz que Onda 2 está aberta, sugira isso ao Antigravity"
Owner (ponte operacional)
    → repassa os prompts entre os agentes
```

---

*Atualizado automaticamente pelo agente Antigravity a cada rodada.*
