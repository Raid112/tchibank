# TchiBank

PWA de controle de gastos mensais contra uma meta. 100% client-side, sem backend, hospedado no GitHub Pages. Sincroniza entre dispositivos via [JSONBin](https://jsonbin.io).

**App:** https://raid112.github.io/tchibank/

## Funcionalidades

- Gastos do mês vs meta (editável por mês, default R$1500) com barra de progresso e projeção
- Lançamento por valor, data, categoria (reutilizável ou nova) e observação
- Navegação entre meses, editar/excluir com confirmação
- Dashes: por categoria, evolução mensal, maiores gastos
- Sync entre 2+ aparelhos

## Setup (primeira abertura)

1. Crie um bin em [jsonbin.io](https://jsonbin.io) com conteúdo inicial `{}`.
2. Abra o app e informe **Master Key**, **Access Key**, **Bin ID** e seu **nome**.

As credenciais ficam **apenas no `localStorage`** do aparelho — nunca no código. Por isso o repositório pode ser público sem expor dados.

## Modelo de sync

JSONBin é a fonte de verdade:

- **Leitura** (abrir / recarregar): `GET` sobrescreve o estado local.
- **Escrita** (lançar / editar / excluir): read-modify-write — `GET` fresco → aplica a operação → `PUT`. Nunca grava o blob local inteiro, evitando que dois aparelhos se sobrescrevam.
- Sem escrita offline (precisa de rede para gravar).

Estrutura do bin:

```json
{ "meta": { "2026-05": 1500 }, "categorias": ["Mercado"], "gastos": { "<id>": { "valor": 0, "categoria": "", "obs": "", "pessoa": "", "data": "<iso>", "mes": "2026-05" } } }
```

## Stack

Vanilla JS, HTML e CSS em arquivo único (`index.html`). PWA com `manifest.json` + `sw.js`. Sem build, sem dependências.

## Rodar localmente

```bash
python -m http.server 8000
```
