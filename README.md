# Site do BaseRoot Linux

Codigo do site do [BaseRoot Linux](https://baseroot.dietpc.org), uma distribuicao
Linux x86_64 minimalista (musl, toybox, mksh).

O site eh HTML puro no estilo 3.2, com frames: sem CSS, sem JavaScript, sem build.

## Estrutura

    index.html     frameset (menu a esquerda, conteudo a direita)
    menu.html      menu lateral
    main.html      pagina inicial: projeto, resumo, novidades, download, fontes
    wiki/          guias e referencia
      index.html   indice da wiki
    CNAME          dominio do site
    LICENSE        dominio publico

## Ver localmente

    python3 -m http.server 8000

Abra http://localhost:8000/ no navegador.

## Convencoes

- HTML 3.2: `<font>`, `<table border>`, `<hr noshade>` etc. Nada de `class`, `<style>` ou scripts.
- Texto em ASCII, sem acentos (por isso "instalacao", "eh").
- Links do menu usam `target="content"`; links dentro das paginas nao precisam de target.
- Paginas da wiki seguem o mesmo modelo: navegacao `<< Wiki | < anterior | proxima >`
  no topo e no rodape, titulo `BaseRoot Wiki - Nome` e a linha "Ultima atualizacao".

## Adicionar uma pagina na wiki

1. Copie uma pagina existente de `wiki/` (por exemplo `qemu.html`) e edite.
2. Liste a pagina em `wiki/index.html`.
3. Se for um guia na sequencia, ajuste os links anterior/proxima das vizinhas.
4. Se quiser, acrescente no `menu.html`.

## Novo lancamento

Atualize em `main.html`: a versao em "Em resumo", a entrada em "Ultimas Novidades"
e a tabela de download. Atualize tambem a versao no topo do `menu.html`.

## Projetos relacionados

- [dumpkg](https://www.github.com/LuuunodotXD/Dump-Package-Manager)
- [Receitas do dumpkg](https://www.github.com/LuuunodotXD/Dump-Package-Recipes)
- [spkg](https://www.github.com/LuuunodotXD/spkg)
- [Repositorio de binarios](https://www.github.com/LuuunodotXD/Dump-Packages)
