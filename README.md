# Wrapper — Gerenciador de Corretores / Ficha do Corretor (GitHub Pages)

```
https://secretaria-de-vendas-alicerce.github.io/corretores/
https://secretaria-de-vendas-alicerce.github.io/corretores/?e=<e-mail>   (o link do convite)
```

## Por que existe

O convite da Agenda e o WhatsApp da secretaria mandavam a `/exec` crua do projeto A
(`script.google.com/macros/s/AKfycbxEl3BD…/exec`): link enorme, com cara de script, e que passa pelo
roteador `/u/N/` do Google — quem tem duas contas logadas abre na conta errada.

Igual ao wrapper da Agenda, este **redireciona** (não embute): o A executa como quem acessa, e
dentro de `<iframe>` o login do Google não acontece (F0, D3). Com o `?e=` do convite, manda para a
`/exec?authuser=<e-mail>`, que abre naquela conta; sem `?e=`, o seletor de conta do Google. Ninguém
confia no `?e=`: a identidade continua sendo a conta Google que o app lê.

**Não usar `AccountChooser?Email=…&continue=<exec>`** (era o desenho até 06/10): o script.google.com
não herda a conta escolhida — cai em `/u/0` ou numa conta Workspace (`/a/<domínio>/…`, "Não foi
possível abrir o arquivo").

Ícones e favicon vêm do kit no Pages do Hub (`hub-alicerce/icones/corretores/`): nada para copiar aqui.

## Publicar (uma vez)

**Publicado em 05/10/2026.** NÃO rode `--source=.` aqui dentro: `wrapper/` mora no repo cephalon, e o
`--push` empurraria o cephalon inteiro (com CPFs) para um repo público. Copie os 3 arquivos para uma
pasta fora do repo, `git init -b main` + commit, e rode de lá (conta `secretaria-de-vendas-alicerce`):

```bash
gh repo create secretaria-de-vendas-alicerce/corretores --public --source=. --push
gh api -X POST repos/secretaria-de-vendas-alicerce/corretores/pages \
  -f "source[branch]=main" -f "source[path]=/"
```

Repo **público**, Pages em `/ (root)`, `.nojekyll` vazio na raiz. Leva ~1 min para o endereço responder.

Sem `gh`: criar o repo `corretores` no GitHub da conta secretaria-de-vendas-alicerce, subir
`index.html` e `.nojekyll`, e em Settings → Pages escolher `main` / `(root)`.

## Depois de publicar

1. **Agenda (projeto A):** `LINK_FICHA` em `agenda-comercial/app/Config.gs` já aponta para este
   endereço → push + nova versão no mesmo deployment (`-i`).
2. **Gerenciador (projeto A):** `LINK_PUBLICO` em `CADASTRO DE CORRETORES/app/Config.gs` já aponta
   para cá → push + nova versão no mesmo deployment (`-i`).

Publique o Pages **antes** dos dois apps: senão o convite leva para uma página 404.

## Quando mexer

- **A `/exec` mudou** (deployment novo em vez de `-i <mesmo id>`, Regra 30): trocar em `index.html`
  (2 lugares: o `EXEC` do script e o link do `<noscript>`).
- **O endereço do Pages mudou:** trocar `LINK_FICHA` (Agenda) e `LINK_PUBLICO` (Gerenciador).

`agenda-comercial/tests/wrapper.test.js` cobra: a `/exec` deste wrapper é uma só, bem formada, sem
iframe, e o convite da ficha sai por aqui com o `?e=`.
