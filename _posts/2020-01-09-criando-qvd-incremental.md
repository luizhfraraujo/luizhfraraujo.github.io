---
layout: post
title: QlikSense - Dados incrementais em QVD (Insert, Update)
image: /img/hello_world.jpeg
category: QlikSense
---

Olá!

Feliz 2020!

Neste post vamos ver como criar armazenar registros em QVD's no QlikSense, e como atualizar ou inserir estes dados tornando assim uma carga incremental.

A carga incremental consiste em trazer somente dados que foram atualizados ou inseridos após determinada data. Para isto, o registro que você for trabalhar deverá possuir informação sobre data de alteração / inclusão.

Vamos trabalhar aqui com o modelo "inline", onde os dados serão criados diretamente no script do QlikSense.

Vamos lá!

Primeiro, vamos criar uma tabela inline de tarefas, com os campos <b>id, descricao, data_criacao, data_alteracao</b>:

Script:
```bash
Tarefas:
Load * Inline [
"id","descricao","data_criacao","data_alteracao"
1, "Carregar dados","08/01/2020","08/01/2020"
2, "Tratar Dados","08/01/2020","08/01/2020"
];

exit Script;
```

![Tabela](/img/2020-01-09-20.16.00.png)


Grande Abraço!

Luiz Henrique Araújo.
