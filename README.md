# Site do BaseRoot Linux

Código do site do [BaseRoot Linux](https://baseroot.dietpc.org), uma distribuição
Linux x86_64 minimalista (musl, toybox, mksh).

O site é HTML puro no estilo 3.2, com frames: sem CSS, sem JavaScript e sem etapa de build.

## Estrutura

    index.html          frameset (menu à esquerda, conteúdo à direita)
    menu.html           menu lateral
    main.html           página inicial: projeto, resumo, novidades, download, fontes
    wiki/               guias e referência
      index.html        índice da wiki
      gravar-iso.html, qemu.html, primeiro-boot.html, instalacao.html
      usuarios.html, pacotes.html, rede.html, servicos.html, relogio-teclado.html, ssh.html
      sistema.html, pacotes-lista.html
      problemas.html
    CNAME               domínio do site
    LICENSE             domínio público

## Ver localmente

    python3 -m http.server 8000

Abra http://localhost:8000/ no navegador. Abra sempre pelo `index.html`, senão o menu não aparece.

## Como contribuir

Contribuições são bem-vindas: correções de texto, comandos desatualizados, páginas novas na wiki.

- **Erro no site ou na wiki:** abra uma *issue* ou um *pull request* neste repositório.
- **Bug no dumpkg, no spkg ou numa receita de pacote:** abra a *issue* no repositório certo (veja [Projetos relacionados](#projetos-relacionados)).

### Regras do HTML

- Use só o que existe no HTML 3.2: `<font>`, `<table border>`, `<hr noshade>`, `<pre>`, `<code>` etc.
  Nada de `class`, `<style>`, `<script>`, `<div>` com layout ou tags do HTML5.
- Cores e fundo ficam nos atributos do `<body>`; copie-os de uma página existente.
- **O texto das páginas é ASCII, sem acentos** (`instalacao`, `eh`, `nao`), e não há `<meta charset>`.
  Isso vale só para os arquivos `.html`; este README pode ter acentos.
  Use entidades para os símbolos do HTML: `&lt;`, `&gt;`, `&amp;`.
- Mantenha a indentação de 4 espaços e as linhas de código dentro de `<pre>`.
- Páginas novas começam com `<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">`.
  O `index.html` é a exceção, pois usa o doctype Frameset.

### Links e frames

- O menu abre tudo no frame da direita: os links do `menu.html` levam `target="content"`.
- Links dentro das páginas **não** levam `target`; ficam no mesmo frame.
- Links relativos: dentro de `wiki/`, use `../main.html#download` para voltar ao projeto.
- Links externos usam `target="_blank"`.
- As âncoras são `<a name="nome"></a>` antes do título. O menu e outras páginas dependem delas;
  se renomear uma, procure com `grep -rn 'nome' .` e corrija os links.

### Modelo de página da wiki

Toda página de `wiki/` segue este modelo:

    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
    <html>
    <head>
        <title>BaseRoot Wiki - Nome da página</title>
    </head>
    <body bgcolor="#FFFFFF" text="#000000" link="#B30000" vlink="#550000">

        <p><font size="2"><a href="index.html">&lt;&lt; Wiki</a> | <a href="anterior.html">&lt; Anterior</a> | <a href="proxima.html">Próxima &gt;</a></font></p>
        <h2>Nome da página</h2>
        <hr size="2" noshade>
        <p>Introdução.</p>

        <h3>Seção</h3>
        <p>Texto.</p>
        <pre>comando</pre>

        <hr size="1" noshade>
        <p><font size="2"><a href="index.html">&lt;&lt; Wiki</a> | <a href="anterior.html">&lt; Anterior</a> | <a href="proxima.html">Próxima &gt;</a></font></p>
        <p><font size="2"><i>Ultima atualizacao: DD/MM/AAAA</i></font></p>

    </body>
    </html>

(Na página real, escreva "Proxima" sem acento.)

Padrões de conteúdo:

- Um `<h2>` por página (o título), seguido de `<hr size="2" noshade>`. As seções são `<h3>`, sem `<hr>` depois.
- Comandos e saídas em `<pre>`; comandos, pacotes e opções no meio do texto em `<code>`;
  caminhos de arquivo em `<i>`.
- Tabelas: `width="100%" bgcolor="#FAFAFA" border="1" cellpadding="5" cellspacing="0"`;
  cabeçalho com `<tr bgcolor="#E8E8E8">` e `<b>`.
- Atualize a linha "Ultima atualizacao" quando mudar o conteúdo.

### Adicionar uma página na wiki

1. Copie uma página existente de `wiki/` (por exemplo `qemu.html`) e edite.
2. Liste a página em `wiki/index.html`, no grupo certo (Começando, Configurar e usar, Referência, Ajuda).
3. Se for um guia da sequência, ajuste os links anterior/próxima das páginas vizinhas.
   A ordem atual é: gravar-iso, qemu, primeiro-boot, instalacao, usuarios, pacotes, rede,
   servicos, relogio-teclado, ssh, problemas.
4. Se a página merecer atalho, acrescente no `menu.html`.

### Adicionar um problema em "Problemas comuns"

Em `wiki/problemas.html`, entradas de sistema, instalação e SSH vão na tabela (sintoma, causa provável, solução).
Wi-Fi, Firefox, pacotes e outras dúvidas ficam nas seções abaixo, no formato título em negrito e lista de passos.

### Novo lançamento da ISO

1. `main.html`: versão em "Em resumo", nova entrada em "Ultimas Novidades" (a mais recente primeiro)
   e uma linha na tabela de download (versão, data e link).
2. `menu.html`: número da versão no topo.
3. Páginas que mencionam versões de componentes: `wiki/sistema.html` e `wiki/pacotes-lista.html`
   (ajuste também a contagem total de pacotes, que aparece em `main.html`, `wiki/index.html` e `wiki/pacotes-lista.html`).
4. Revise a data em "Ultima atualizacao" das páginas alteradas.

### Antes de enviar

Confira se não ficou link quebrado, âncora faltando ou caractere fora do ASCII nas páginas.
Na raiz do repositório:

    python3 - <<'PY'
    import re, os, glob
    for f in sorted(glob.glob('*.html') + glob.glob('wiki/*.html')):
        s = re.sub(r'<!--.*?-->', '', open(f).read(), flags=re.S)
        for h in re.findall(r'href="([^"]+)"', s):
            if h.startswith('http'): continue
            path, _, frag = h.partition('#')
            t = os.path.normpath(os.path.join(os.path.dirname(f), path)) if path else f
            if not os.path.exists(t) or (frag and 'name="%s"' % frag not in open(t).read()):
                print('quebrado:', f, '->', h)
    PY

    grep -nP '[^\x00-\x7F]' *.html wiki/*.html

Nenhuma das duas verificações deve imprimir nada. Depois, abra o site pelo `index.html` e clique nos links do menu
e na navegação anterior/próxima das páginas que você mexeu.

## Projetos relacionados

- [dumpkg](https://www.github.com/LuuunodotXD/Dump-Package-Manager): gerenciador de pacotes
- [Receitas do dumpkg](https://www.github.com/LuuunodotXD/Dump-Package-Recipes)
- [spkg](https://www.github.com/LuuunodotXD/spkg): gerenciador de pacotes mínimo
- [Repositório de binários](https://www.github.com/LuuunodotXD/Dump-Packages) (dumpkg e spkg)

## Licença

Domínio público. Veja o arquivo `LICENSE`.
