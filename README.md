# Checkout MB WAY + Multibanco — site

Site publicado no GitHub Pages a partir do `public/index.html` do projeto.

## 🔗 Links

| Arquivo | URL | O que é |
|---|---|---|
| `index.html` | https://pedido-atrasado.github.io/broski-checkout-demo/ | **O index original do projeto, sem alteração** |
| `simulado.html` | https://pedido-atrasado.github.io/broski-checkout-demo/simulado.html | Mesmo site, com o backend simulado no browser — o formulário **funciona até ao fim** |

## Diferença entre os dois

**`index.html`** — é literalmente o `public/index.html` do projeto. Renderiza igual.
Mas o botão **Doar dá erro**: ele faz `fetch('/api/checkout')`, e no GitHub Pages
não existe servidor para responder — o Pages só entrega ficheiros. Aparece
*"Erro de rede. Tente novamente."* Sem backend, nada acontece.

**`simulado.html`** — mesma página, mesmo CSS, mesmo fluxo, mas com o backend
replicado em JS dentro do próprio ficheiro. Dá para percorrer o fluxo todo:
formulário → espera do MB WAY → confirmação, e o voucher Multibanco. Reproduz as
regras do servidor (valor do `CATALOG`, telemóvel `^9[1236]\d{7}$`, limites MB WAY,
409 de duplicado, MB WAY confirma em ~6 s, Multibanco sem polling). Está marcado
como DEMO, com `noindex` e aviso de que nada é cobrado.

## Por que o site não funciona sozinho

O projeto é um servidor Node 22 + Express (`server.js`, rotas `/api/checkout`,
`/api/orders/:ref/status`, `/webhooks/broski`). O GitHub Pages **não executa
código** — só serve ficheiros estáticos. Para o checkout processar de verdade,
o projeto precisa de um host com Node (Railway, Render, VPS) e das chaves reais
(`BROSKI_SECRET_KEY`, `BROSKI_WEBHOOK_SECRET`), com `PUBLIC_URL` HTTPS a bater
**exatamente** com a URL registada no painel Broski.

O código-fonte completo está no repositório privado `broski-checkout`.

## Rodar localmente

```bash
python -m http.server 8080
```
