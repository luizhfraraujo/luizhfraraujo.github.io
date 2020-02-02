---
layout: post
title: Criando um projeto com Ionic, Angular e Firebase
image: https://luizhfraraujo.github.io/img/posts/ionic_firebase.gif
tags: [Ionic, Firebase, Angular]
share-img: https://luizhfraraujo.github.io/img/posts/ionic_firebase.gif
---

Olá!

Vamos iniciar uma série de posts sobre desenvolvimento híbrido utilizando Ionic, Angular e Firebase.

Caso você não conheça as tecnologias, segue os sites e descrições oficiais:

<a href="https://ionicframework.com/">Ionic</a>
<br/>
<i>"Aplicativos rápidos e bonitos
O Ionic Framework é o kit de ferramentas de interface do usuário móvel gratuito e de código aberto para o desenvolvimento de aplicativos de plataforma cruzada de alta qualidade para iOS, Android e Web nativos - tudo a partir de uma única base de código.</i>

<i>Crie com componentes de interface do usuário intuitivos que aceleram o desenvolvimento de aplicativos e podem ser implantados praticamente em qualquer lugar."</i>

<a href="https://firebase.google.com/">Firebase</a>
<br/>

<p>
<i>
<b>Crie aplicativos rapidamente, sem precisar gerenciar a infraestrutura</b>
Com as funcionalidades do Firebase, como análises, bancos de dados, mensagens e relatórios de erros, você tem mais agilidade e pode se concentrar nos seus usuários.
</i>
</p>
<p>
<i>
<b>Feito pelo Google, usado pelos melhores aplicativos</b>
O Firebase foi criado sobre a infraestrutura do Google e faz o escalonamento automático, até mesmo para os maiores apps.
</i>
</p>
<p>
<i>
<b>Uma plataforma com produtos que funcionam melhor juntos</b>
Os produtos do Firebase funcionam muito bem sozinhos, mas têm um resultado ainda melhor quando compartilham dados e insights entre si.
</i>
</p>



Vamos ao código!

Inicie instalando o Ionic via npm:

```bash
npm install -g @ionic/cli
```

Com o Ionic instalado, vamos iniciar um novo projeto vazio.

```bash
ionic start ionic-firebase-starter
```

![IonicFirebase](/img/posts/2020-02-01.01.png)

Aparecerá uma lista para selecionarmos qual framework vamos utilizar. Neste caso, vamos utilizar o Angular.
Mova com a seta do seu teclado para Angular e aperte enter.

![IonicFirebase](/img/posts/2020-02-01.02.png)

Em seguida, o Ionic apresenta opções de templates para iniciarmos o projeto. Vamos escolher o "Blank".

![IonicFirebase](/img/posts/2020-02-01.03.png)

Vamos agora habilitar o Capacitor para depois gerar os builds do nosso app. Capacitor é o novo framework do Ionic para criação de apps para Android, iOS e Electron (Desktop). Dê uma conferida no Capacitor <a href="https://capacitor.ionicframework.com/">clicando aqui</a>. Para habilitar, aperte <b>Y</b> e dê enter.


A partir desde momento, o CLI do Ionic vai instalar os pacotes necessários para o funcionamento do mesmo.

Quando acabar, aparecerá uma mensagem semelhante a imagem abaixo:

![IonicFirebase](/img/posts/2020-02-01.04.png)

Vamos então acessar o projeto ionic-firebase-starter com um <b>cd ionic-firebase-starter</b> e executar um <b>code .</b> para abrir o Visual Studio Code. 

Com o VSCode aberto, podemos ver a estrutura que o Ionic cria para a gente.

![IonicFirebase](/img/posts/2020-02-01.05.png)

Vamos dar uma passada em alguns items:

<b>src</b> Pasta principal contendo todo o conteúdo do aplicativo
<b>src/app:</b> Código da nossa aplicação
<b>src/assets:</b> Estilos, bibliotecas, imagens
<b>src/environments:</b> Variáveis de ambientes da nossa aplicação. Quando executamos, ele pega um tipo de arquivo de ambiente. Quando geramos um build, ele pega o outro arquivo environment.prod
<b>src/theme:</b> Pasta contendo as variáveis de tema da aplicação
<b>angular.json:</b> Configurações do Angular
<b>capacitor.config.json:</b> Configurações do Capacitor
<b>ionic.config.json:</b> Configurações do Ionic
<b>package.json:</b> Arquivo criado pelo NPM com as configurações do projeto.

Certo, com esta breve passada pelos principais arquivos, vamos rodar nosso projeto. Para isto execute o seguinte comando dentro da pasta do projeto:

```bash
ionic serve
```

Este comando vai iniciar o servidor e abrir o seu navegador com a url http://localhost:8100

Vai aparecer a seguinte tela:

![IonicFirebase](/img/posts/2020-02-01.06.png)

Vemos que ele abriu em tela inteira. Vamos agora tentar simular como seria em um dispositivo usando o próprio navegador. Para isto aperte F12 (Estou usando Windows e Google Chrome) ou acesse Menu -> Mais Ferramentas -> Ferramentas do Desenvolvedor. 

Vai abrir a seguinte tela:

![IonicFirebase](/img/posts/2020-02-01.07.png)

Clique agora la em cima, onde (no meu caso) está Responsive e selecione o dispositivo que queira testar. No meu caso vou  selecionar iPhone X.
![IonicFirebase](/img/posts/2020-02-01.08.png)

![IonicFirebase](/img/posts/2020-02-01.09.png)

Veja que a tela foi redimensionada para a resolução do iPhone X, mas manteve ainda o Material Design do Android. Vamos atualizar a página para que o Ionic entenda e consiga exibir os components do iOS

![IonicFirebase](/img/posts/2020-02-01.10.png)

Veja que agora o título foi para o centro e a fonte mudou. Isso porque o Ionic ja entende qual plataforma que ele está e adapta o visual dos seus componentes para aquela plataforma.
- Desktop, Android e Web utilizará o Material Design
- iOS utilizará o Cupertino (Padrão do iOS)

Para não ficar muito extenso, no próximo post vamos começar a codar nosso projeto adicionando a biblioteca AngularFire e conectando com o Firebase.

Este projeto estará todo disponível em:
<a href="https://github.com/luizhfraraujo/ionic-firebase-starter">https://github.com/luizhfraraujo/ionic-firebase-starter
</a>

Qualquer dúvida, sugestão, entre em contato comigo!

<a href="https://github.com/luizhfraraujo">https://github.com/luizhfraraujo<a>

Grande Abraço!

Luiz Henrique Araújo.
