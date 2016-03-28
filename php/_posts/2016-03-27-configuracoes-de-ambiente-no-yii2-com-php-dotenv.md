---
layout: post
title: Configurações de ambiente no Yii2 com PHP dotenv
comments: true
---

Frequentemente, configurações mudam conforme o ambiente em que a aplicação roda. O Yii2 fornece recursos para lidar com estas diferenças, mas não estimula o desenvolvedor a extrair estas informações do código para variáveis de ambiente, o que seria mais seguro no caso de tokens, senhas e outros dados sensíveis. É aqui que o PHP dotenv pode ajudar.
