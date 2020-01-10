---
layout: post
title: QlikSense - Dados incrementais em QVD (Insert, Update)
image: https://help.qlik.com/img/logos/Qlik-Help-2019.svg
category: QlikSense
---

Olá!

Feliz 2020!

Recentemente precisei desenvolver um script que realizaria cargas incrementais em um QVD. 
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
1, "Carregar dados da fonte","08/01/2020","08/01/2020"
2, "Tratar Dados","08/01/2020","08/01/2020"
];

exit Script;
```

![Tabela](/img/posts/2020-01-09-20.16.00.png)

Agora nós vamos criar uma nova conexão de pasta, para que possamos salvar nosso QVD.

![Tabela](/img/posts/2020-01-09-20.19.40.png)

![Tabela](/img/posts/2020-01-09-20.19.49.png)

Escolha um local e o nome da sua conexão e vamos então salvar nossa tabela em nosso QVD.

Script:
```bash
Tarefas:
Load * Inline [
"id","descricao","data_criacao","data_alteracao"
1, "Carregar dados da fonte","08/01/2020","08/01/2020"
2, "Tratar Dados","08/01/2020","08/01/2020"
];

// Armazena as tarefas no QVD
STORE Tarefas INTO '[lib://PathQVDTeste (qlikserver_luiz)/tarefas.qvd]';

exit Script;
```

Certo, neste momento temos um QVD criado, que a todo momento em que rodarmos a carga, vai ser limpo e salvo novamente os dados. Este é o carregamento padrão do Qlik, o Insert.

Para que possamos realizar tanto o Insert quanto o Update dos dados, vamos fazer o seguinte ajuste:

Script:
```bash
Tarefas:
Load * Inline [
"id","descricao","data_criacao","data_alteracao"
1, "Carregar dados da fonte","08/01/2020","08/01/2020"
2, "Tratar Dados","08/01/2020","08/01/2020"
3, "Salvar Dados em um QVD","08/01/2020","08/01/2020"
];

// Vamos verificar se existe o QVD Tarefas
if not isnull(QvdCreateTime('[lib://PathQVDTeste (qlikserver_luiz)/tarefas.qvd]')) then
    // caso o QVD exista, vamos então concatenar os dados que ja existem mas ignorando 
    // os registros que estão em nosso load da tabela, ou seja, somente os ids que 
    // não existirem em nosso QVD que serão carregados.

	Concatenate LOAD * FROM '[lib://PathQVDTeste (qlikserver_luiz)/tarefas.qvd]'(qvd) WHERE NOT EXISTS(id);
End If;


// Armazena as tarefas da tabela concatenados com o QVD no QVD novamente
STORE Tarefas INTO '[lib://PathQVDTeste (qlikserver_luiz)/tarefas.qvd]';

exit Script;
```

Vamos no modo de visualização do modelo de dados para ver como está nossa tabela

![Tabela](/img/posts/2020-01-09-20.39.50.png)

![Tabela](/img/posts/2020-01-09-20.39.14.png)

Podemos ver que a nossa informação criada em nossa tabela inline está carregando corretamnete.

Vamos confirmar que o que estamos carregando é o que está salvo em nosso QVD, para isso faça o seguinte:

Script:
```bash
Tarefas:
Load * Inline [
"id","descricao","data_criacao","data_alteracao"
1, "Carregar dados da fonte","08/01/2020","08/01/2020"
2, "Tratar Dados","08/01/2020","08/01/2020"
3, "Salvar Dados em um QVD","08/01/2020","08/01/2020"
];

// Vamos verificar se existe o QVD Tarefas
if not isnull(QvdCreateTime('[lib://PathQVDTeste (qlikserver_luiz)/tarefas.qvd]')) then
    // caso o QVD exista, vamos então concatenar os dados que ja existem mas ignorando 
    // os registros que estão em nosso load da tabela, ou seja, somente os ids que 
    // não existirem em nosso QVD que serão carregados.

	Concatenate LOAD * FROM '[lib://PathQVDTeste (qlikserver_luiz)/tarefas.qvd]'(qvd) WHERE NOT EXISTS(id);
End If;


// Armazena as tarefas da tabela concatenados com o QVD no QVD novamente
STORE Tarefas INTO '[lib://PathQVDTeste (qlikserver_luiz)/tarefas.qvd]';
DROP TABLE Tarefas;

TarefasSalvas:
LOAD * FROM '[lib://PathQVDTeste (qlikserver_luiz)/tarefas.qvd]'(qvd);

exit Script;
```

Neste script nos deletamos a tabela Tarefas para garantir que vamos carregar a do QVD, e vamos carregar agora o QVD e colocar o nome de TarefasSalvas.

![Tabela](/img/posts/2020-01-09-20.44.46.png)

<h2>Inserindo e atualizando dados</h2>

O que vamos fazer é adicionar novos dados e atualizar um registro existente.
Para isto, vamos criar o seguinte script, que vai obedecer agora o campo <b>data_alteracao</b>:

Script:
```bash

Let vLastUpdatedDate = Text(Date(Reloadtime(),'DD/MM/YYYY'));

trace Data da Última atualização: $(vLastUpdatedDate);

Tarefas:
Load * Inline [
"id","descricao","data_criacao","data_alteracao"
1, "Carregar dados da fonte","08/01/2020","08/01/2020"
2, "Tratar Dados","08/01/2020","08/01/2020"
3, "Salvar Dados em um QVD e carregar em outra tabela","08/01/2020","09/01/2020"
4, "Inserir um novo dado no QVD","09/01/2020","09/01/2020"
] WHERE data_alteracao = '$(vLastUpdatedDate)';

// Vamos verificar se existe o QVD Tarefas
if not isnull(QvdCreateTime('[lib://PathQVDTeste (qlikserver_luiz)/tarefas.qvd]')) then
    // caso o QVD exista, vamos então concatenar os dados que ja existem mas ignorando 
    // os registros que estão em nosso load da tabela, ou seja, somente os ids que 
    // não existirem em nosso QVD que serão carregados.

	Concatenate LOAD * FROM '[lib://PathQVDTeste (qlikserver_luiz)/tarefas.qvd]'(qvd) WHERE NOT EXISTS(id);
End If;


// Armazena as tarefas da tabela concatenados com o QVD no QVD novamente
STORE Tarefas INTO '[lib://PathQVDTeste (qlikserver_luiz)/tarefas.qvd]';
DROP TABLE Tarefas;

TarefasSalvas:
LOAD * FROM '[lib://PathQVDTeste (qlikserver_luiz)/tarefas.qvd]'(qvd);

exit Script;
```

Veja que adicionamos a tarefa com id 4 e atualizamos a descricão do id 3 para
"Salvar Dados em um QVD e carregar em outra tabela". Adicionamos no início uma variável para armazenar a data da última carga, e também chamamos o "trace" para exibir uma mensagem em nossa carga.

![Trace](/img/posts/2020-01-09-21.23.08.png)

Após executar o script, teremos os seguinte resultado:


![Trace](/img/posts/2020-01-09-21.23.34.png)

Espero que tenha ajudado. Qualquer dúvida, sugestão, entre em contato comigo!

<a href="https://github.com/luizhfraraujo">https://github.com/luizhfraraujo<a>

Script final:
<a href="https://github.com/luizhfraraujo/carga-incremental-qliksense">https://github.com/luizhfraraujo/carga-incremental-qliksense</a>

Grande Abraço!

Luiz Henrique Araújo.
