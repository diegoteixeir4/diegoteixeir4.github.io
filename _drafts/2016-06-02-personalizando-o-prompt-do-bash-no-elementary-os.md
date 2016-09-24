---
layout: post
title: Alterando o prompt do terminal no Elementary OS
comments: true
---

## Seu terminal é o Bash?

O Bash é o terminal padrão do Elementary OS, da maioria das distribuições do Linux e do OSX. Caso queira conferir se ele é o terminal ativo, digite `echo $0`. Se o resultado for `/usr/bin/bash`, tudo certo.

## Testando prompts

A configuração do prompt é armazenada na variável de ambiente `PS1`. Para ver o valor atual, digite `echo $PS1`. O valor padrão será algo assim:

```
\[\e]0;$(print_title)\a\]${debian_chroot:+($debian_chroot)}\u@\h:\w\$
```

A primeira parte, `\[\e]0;$(print_title)\a\]`, é adicionada automaticamente, então não precisa usa-lá nos seus testes. O que ela faz é alterar o título da janela ou da aba do terminal.

A seguir, a expressão `${debian_chroot:+($debian_chroot)}`, exibe um indicador na frente do prompt quando se está em um ambiente chroot. Pode omitir no seu teste, mas deixe no resultado final, pode ser útil.

Enfim, a parte que queremos personalizar é a que sobrou: `\u@\h:\w\$`. É esta expressão que descreve o prompt padrão, `user@hostname:/active/dir$`, usando sequências de escape.

Para alterar, basta utilizar `PS1='...'`.

### Sequências de escape do Bash

Sequência | Descrição
----------|----------
\d | a data no formato "Dia da semana Mês Dia" (ex., "Qui Jan 18")
\e | o caractere ASCII esc (033)
\h | o hostname da máquina antes do "."
\H | o hostname da máquina
\j | o número de processos atualmente gerenciados pelo shell
\l | o nome do dispositivo do terminal
\n | nova linha
\r | retorno do cursor
\s | o nome do shell
\t | data atual no formato de 24-horas HH:MM:SS
\T | data atual no formato de 12-horas HH:MM:SS
\@ | data atual no formato de 12-hour am/pm
\u | o nome do usuário atual
\v | a versão do bash (ex., 2.02)
\V | a liberação, versão + patchlevel (ex., 2.00.0)
\w | o diretório atual
\W | o nome base do diretório corrente
\! | o número no histórico do comando atual
\# | o número do comando desde que aberto o terminal
\$ | se o usuário tiver UID igual a 0, ou seja, o usuário atual é o root, aparecerá o caractere #, ou então $ para todos os outros usuários.
\\ | uma barra invertida
\[ | inicio de uma seqüência de caracteres que não serão mostrados na tela. ex. colocar cor nas letras)
\] | fim de uma seqüência de caracteres que não serão mostrados na tela.

```bash
# uncomment for a colored prompt, if the terminal has the capability; turned
# off by default to not distract the user: the focus in a terminal window
# should be on the output of commands, not on the prompt
#force_color_prompt=yes

if [ -n "$force_color_prompt" ]; then
    if [ -x /usr/bin/tput ] && tput setaf 1 >&/dev/null; then
        # We have color support; assume it's compliant with Ecma-48
        # (ISO/IEC-6429). (Lack of such support is extremely rare, and such
        # a case would tend to support setf rather than setaf.)
        color_prompt=yes
    else
        color_prompt=
    fi
fi

if [ "$color_prompt" = yes ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
fi
unset color_prompt force_color_prompt
```

Se quiser usar cores, remover o comentário da linha `force_color_prompt=yes`.
