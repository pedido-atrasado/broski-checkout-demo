# Doações — Checkout MB WAY + Multibanco (DEMO)

**Site estático de demonstração** do frontend do checkout. Publicado no GitHub Pages.

> ⚠️ **DEMO.** Os pagamentos são **simulados** — nenhuma doação é cobrada, processada
> ou recebida. As referências Multibanco mostradas não são pagáveis. O link é público,
> mas está marcado `noindex` e não é para divulgação a doadores.

## O que isto é (e o que não é)

| | |
|---|---|
| ✅ É | O frontend real (`public/index.html` do projeto), com o mesmo visual e o mesmo fluxo |
| ✅ É | 100% estático — HTML + CSS + JS, zero build, zero servidor |
| ❌ Não é | O sistema em produção. Não fala com a API Broski, não tem chaves, não processa dinheiro |

O backend está **simulado dentro do próprio `index.html`** (bloco `SIM`). As regras
foram replicadas para a demo se comportar como o servidor de verdade:

- o valor vem sempre do "servidor" (`CATALOG`), nunca do corpo do pedido — só `doacao-livre` é validado
- telemóvel português validado por `^9[1236]\d{7}$`
- limites de MB WAY: €0,50 a €5.000
- MB WAY: `pending` → `paid` (como se o webhook `order.paid` tivesse chegado)
- Multibanco: `awaiting_payment` com entidade/referência, **ecrã final sem polling**
- duplicado: pedido MB WAY pendente para o mesmo número devolve 409 `mbway_pending_for_phone`

## Prazo da simulação

| Método | Comportamento |
|---|---|
| MB WAY | confirma sozinho após ~6 s |
| Multibanco | ecrã final; entidade `11249`, referência gerada, validade 2 dias |

## Por que aqui e não no repositório principal

GitHub Pages **serve apenas ficheiros estáticos**. O projeto real é um servidor
Node.js + Express (`server.js`, rotas `/api/checkout`, `/api/orders/:ref/status`,
`/webhooks/broski`), que o Pages não executa. Por isso:

- **este repositório** = só o site, público, para ver o fluxo num link `github.io`
- **o código-fonte** = repositório privado, precisa de um host com Node 22
  (Railway, Render, VPS) e de chaves reais

Para produzir de verdade: `BROSKI_SECRET_KEY`, `BROSKI_WEBHOOK_SECRET` e um
`PUBLIC_URL` HTTPS que bata **exatamente** com a URL registada no painel Broski.

## Rodar localmente

```bash
python -m http.server 8080
# http://localhost:8080
```

Qualquer servidor de ficheiros estáticos serve. Não há dependências.
