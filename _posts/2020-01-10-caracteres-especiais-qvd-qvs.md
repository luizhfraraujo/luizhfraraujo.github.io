---
layout: post
title: QlikSense - Caracteres especiais em colunas utilizando QVS com VSCode
image: https://help.qlik.com/img/logos/Qlik-Help-2019.svg
tags: [QlikSense, Script]
share-img: https://help.qlik.com/img/logos/Qlik-Help-2019.svg
---

Olá!

Você já precisou salvar colunas com caracteres especiais? Ex: Código, Ação, Data_Alteração.

Isto funciona corretamente com o Qliksense script direto no aplicativo, mas, e se você gerar em QVS? Já testou?

Eu estava separando os scripts que criei no aplicativo e salvando eles em arquivos QVS utilizando Visual Studio Code, quando me deparei com a seguinte situação: Colunas com caracteres especiais estavam sendo salvas diferente do que estava no script. Isso é muito comum quando usamos o formato ANSI ao invés do UTF-8, mas o meu VSCode estava configurado para salvar em UTF-8.

Nisso, peguei o script, coloquei ele no QlikSense e ele funcionava corretamente, gerando a coluna correta. Colocava no script QVS, e ele dava problema.

Exemplo:

```bash
Tarefas:
Load * Inline [
"código","descrição","data_criação","data_alteração"
1, "Carregar dados da fonte","08/01/2020","08/01/2020"
2, "Tratar Dados","08/01/2020","08/01/2020"
];

exit Script;
```

Quando executamos este script no QlikSense, cria uma tabela com as tarefas e podemos perceber que as colunas estão com os nomes iguais ao informado no script.

![Tabela](/img/posts/2020-01-10-17-48-00.png)

Agora nos vamos salvar este script como um arquivo QVS e incluir ele no nosso script de carga.

Script no Visual Studio Code (utilizo a extensão <a href="https://github.com/Gimly/vscode-qlik.git">VSCode Qlik</a> para adicionar suporte a QVS):
![Tabela](/img/posts/2020-01-10-17-48-05.png)

Arquivo salvo na pasta:
![Tabela](/img/posts/2020-01-10-17-48-06.png)

Script:

```bash
// lib://ScriptFiles (qlikserver_luiz) é o nome da conexão da pasta que criei.

$(Include=[lib://ScriptFiles (qlikserver_luiz)/dados/tarefas.qvs]);
exit script;
```

Ao executar o script e irmos a aba de dados, vemos que os caracteres estão diferentes:
![Tabela](/img/posts/2020-01-10-17-48-07.png)

Isso acontece pois o QlikSense precisa que os arquivos QVS estajam no formato <b>UTF-8 with BOM</b>. Caso esteja em outro formato, como o UTF-8 padrão, ele vai carregar com os caracteres incorretos.

Para corrigir isto vamos salvar o nosso script como UTF-8 with BOM:

Vá no canto inferior direito e clique no formato:
![Tabela](/img/posts/2020-01-10-17-48-08.png)

ele vai abrir a caixa em cima para você selecionar o que fazer:
![Tabela](/img/posts/2020-01-10-17-48-09.png)

Salve no formato correto
![Tabela](/img/posts/2020-01-10-17-48-10.png)

Ao realizar a carga novamente, você verá que as colunas estarão corrigidas!

Espero que tenha ajudado. Qualquer dúvida, sugestão, entre em contato comigo!

<a href="https://github.com/luizhfraraujo">https://github.com/luizhfraraujo<a>

Grande Abraço!

Luiz Henrique Araújo.
