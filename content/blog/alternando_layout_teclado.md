+++
categories  = ["sistemas", "dicas"]
date        = 2025-09-05
draft       = false
tags        = ["openbsd", "teclado", "produtividade", "til"]
title       = "TIL: Alternando Layout do Teclado"
description = """
Hoje aprendi (TIL) como deixar minha rotina mais fluida alternando entre
Português Brasileiro e Inglês no teclado."""
+++

Hoje aprendi (TIL) como deixar minha rotina mais fluida alternando
entre **Português Brasileiro** e **Inglês** no teclado. Meu teclado por padrão
é _americano_, ou seja, não possui a tecla **ç** que é comum nos teclados "br".

No meu dia a dia preciso escrever em português quando estou em reuniões,
chats e e-mails, mas prefiro digitar em inglês quando estou
codificando ou escrevendo documentos técnicos.

A solução anterior era digitar o comando [setxkbmap][0] todas vez que precisava
alterar o layout do teclado, cansado disso fiz um script em shell que
alternava entre os layouts mesmo eu configurando uma tecla de atalho para
chamar esse script achei que deveria existir uma forma melhor.

A solução foi configurar um atalho simples para alternar entre os dois
layouts de teclado direto no [setxkbmap][0].

Na pagina manual na explicação sobre a opção `layout` existe uma passagem
que explica exatamente o detalhe que estava procurando.

> _"...Multiple layouts can be specified as a comma-separated list."_

## O comando

```bash
setxkbmap -layout 'us,us' -variant ',intl' -option 'grp:win_space_toggle'
```

## O que faz?

- `-layout 'us,us'`: define o layout padrão como 'us' (US) e adiciona
  uma segunda variação também `us`.
- `-variant ',intl'`: a segunda opção é configurada como _US International_,
  permitindo acentos para português.
- `-option 'grp:win_space_toggle'`: defino o atalho `Win + Espaço` para
  alternar entre os layouts.

## Extra

Caso queira utilizar outras teclas de atalho para alternar entre layouts no
manual [xkeyboard-config][2] possui uma seção com todas possibilidades.

## Links

- [Manual setxkbmap][0]
- [Manual xkeyboard-config][1]

[0]: https://man.openbsd.org/setxkbmap
[1]: https://man.openbsd.org/xkeyboard-config
[2]: https://man.openbsd.org/xkeyboard-config#OPTIONS
