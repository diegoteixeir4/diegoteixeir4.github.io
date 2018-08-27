---
title: Como configurar os botões extras do mouse no Ubuntu
date: 2018-08-26 16:30:00
---

Em alguns mouses há botões extras, além dos padrões que já possuem funções bem definidas no X:

- esquerdo: selecionar/ativar um item
- direito: menu de contexto para um item
- scroll/meio: tem a função de colar em entradas de texto e de abrir o item em uma aba ou janela em alguns aplicativos


Para os botões extras normalmente são atribuidas funções padrão ou nenhuma função, mas isso pode ser modificado.

Iremos instalar o xbindkeys, um utilitário para capturar eventos do mouse e teclado no X e iniciar comandos shell, e o xautomation, um pacote com várias ferramentas para automação do X, aqui vamos usar o xte, que automatiza funções de mouse e teclado.


```
$ sudo apt install xautomation xbindkeys
```

A seguir criamos um arquivo de configuração do xbindkeys com as opções padrão, o arquivo é utilizado para o usuário atual somente e fica em ~/.xbindkeysrc.

```
$ xbindkeys -d > $HOME/.xbindkeysrc
```

Agora, precisamos saber como o X identifica os botões extras do mouse. Para isso, vamos utilizar o xev, um utilitário que exibe os eventos do X.

```
$ xev
```

Ao executar, uma janela será exibida, basta clicar com o botão desejado e procurar um evento como esse:

```
ButtonPress event, serial 37, synthetic NO, window 0x1800001,
  root 0x1af, subw 0x0, time 21883921, (67,126), root:(1382,178),
  state 0x10, button 8, same_screen YES

ButtonPress event, serial 37, synthetic NO, window 0x1800001,
  root 0x1af, subw 0x0, time 21883921, (67,126), root:(1382,178),
  state 0x10, button 9, same_screen YES
```

Nesse caso, pressionei os dois botões na lateral do meu mouse. O botão 8 é o de baixo e o 9 o de cima.

Vamos supor que queremos colocar nesses botões as funções das teclas PgDn e PgUp do teclado, respectivamente. O que vamos fazer é usar o xbindkeys para identificar o código dessas teclas.

```
$ xbindkeys -mk
Press combination of keys or/and click under the window.
You can use one of the two lines after "NoCommand"
in $HOME/.xbindkeysrc to bind a key.

--- Press "q" to stop. ---
"(Scheme function)"
    m:0x10 + c:117
    Next
"(Scheme function)"
    m:0x10 + c:112
    Prior
```

Basta executar o comando e executar as teclas das quais deseja capturar o código. Nesse caso, "Next" para PgDn e "Prior" para PgUp.

Agora que temos os códigos das teclas e os números dos botões do mouse. Basta editar o arquivo ~/.xbindkeysrc e adicionar o seguinte ao final do arquivo:

```
#Pagedown
"xte 'key Next'"
b:8

#Pageup
"xte 'key Prior'"
b:9
```

A linha de cima, diz qual comando shell executar e a de baixo para qual tecla ou botão do mouse. Estamos usando o xte para simular o pressionamento das teclas no teclado.

Após alterar o arquivo é preciso reiniciar o daemon do xbindkeys:

```
$ killall xbindkeys
$ xbindkeys ~/.xbindkeysrc
```


