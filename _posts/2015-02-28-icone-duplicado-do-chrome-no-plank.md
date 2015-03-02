---
layout: post
title: Eliminando o ícone duplicado do Chrome no dock do Elementary OS
excerpt: Ao instalar o Chrome no Elementary OS, talvez o lançador não corresponda a janela como esperado, o que faz com que apareça um novo ícone repetido no Plank, o dock padrão do Elementary OS.
---

O dock padrão do Elementary OS é o Plank, aplicação que foi criada pela mesma equipe que desenvolveu o Docky. É um dock simples, leve e de inicialização praticamente instantânea, apesar de faltarem alguns “ajudantes” como paginador de desktops, lixeira etc. presentes em outras aplicações do genêro como o Cairo Dock.

Ao instalar o Chrome no Elementary OS, talvez o lançador não corresponda a janela como esperado, o que faz com que apareça um novo ícone repetido no Plank. Corrigir isto é simples, abra o terminal e digite:

    sudo nano /usr/share/applications/google-chrome.desktop

Neste arquivo, você vai encontrar três grupos de parâmetros: `[Desktop Entry]`, `[NewWindow Shortcut Group]` e `[NewIncognito Shortcut Group]`. Insira em cada um dos grupos a seguinte linha:

    StartupWMClass=Google-chrome-stable

Salve o arquivo pressionando `CTRL+O` e saia pressionando `CTRL+X`. Pronto, isto deve resolver o problema imediatamente.
