---
title: Como instalar fontes no Ubuntu
date: 2018-08-25 17:00:00
description: Instalando fontes em massa no Ubuntu
---

Para instalar uma fonte por vez, o processo é simples: basta clicar com o botão direito sobre o arquivo correspondente, em seguida em "Abrir com o Visualizador de fontes" e então em "Instalar" na janela que abrirá.

Para instalar várias fontes de uma só vez, copie os arquivos para o diretório `~/.fonts` e a seguir execute no terminal:

```
fc-cache -fv
```


