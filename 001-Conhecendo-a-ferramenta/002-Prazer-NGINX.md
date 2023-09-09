

##

git status
git add .
git commit -m "NGINX Alura."
eval $(ssh-agent -s)
ssh-add /home/fernando/.ssh/chave-debian10-github
git push
git status





## Transcrição

[00:00] Antes de instalarmos, configurarmos e utilizarmos o NGINX, antes de realmente conhecermos a parte prática, nós vamos entender um pouco da teoria. O que um NGINX faz e como ele faz. Então, vamos lá!

[00:15] O NGINX é um servidor web. Ele é um serviço, é um programa que roda e que serve para responder requisições web. Então, é muito importante que você entenda como a web funciona. Por isso, o pré-requisito deste treinamento é o curso de HTTP. Você precisa entender o protocolo HTTP para saber qual é o papel do NGINX.

[00:34] Então super simplificando milhares de conceitos que você já deve ter aprendido no curso de HTTP, a web funciona com uma arquitetura cliente-servidor. Temos aqui o cliente e o servidor, então imagine que o cliente é um navegador web; e nós temos um servidor rodando, uma aplicação em Java, em PHP, C Sharp, Python, Ruby ou a linguagem que for.

[00:55] Então, o cliente vai digitar a URL do nosso site: “alura.com.br”, por exemplo, e ele vai fazer uma requisição para o computador que tem o nome “alura.com.br”.

[01:08] Esse computador precisa saber receber essa requisição web e fazer o que tem que fazer - ou exibir o HTML, a imagem, devolver o arquivo CSS - ou então mandar para uma aplicação processar essa requisição. Uma aplicação em Java, em PHP, em Python ou no que for.

[01:25] Essa tarefa de receber a requisição e decidir o que vai fazer, se vai mandar para uma aplicação, se vai devolver algum arquivo direto - essa é a tarefa de um servidor web.

[01:37] Então é aí que entra o NGINX! Esse é o papel dele. O que o NGINX vai fazer dentro de um servidor é ficar ouvindo uma porta. Quando falamos de HTTP, a porta padrão é a “80”. Então se você não digita uma porta e está utilizando HTTP, essa requisição vai cair na porta 80.

[01:57] Então um servidor web fica lá ouvindo, por exemplo, a porta 80 e ele fica esperando alguma requisição ou algum pedido entrar pedido chegar nesse computador. Quando chega, esse servidor web tem que saber o que fazer.

[02:13] Então, ele vai analisar. Se o que você está pedindo parece ser um arquivo de imagem, então não preciso fazer nada com mais ninguém. Eu só vou te devolver essa imagem e o navegador vai exibir para você.

[02:23] Você está me pedindo um arquivo estático, um arquivo de CSS? Então só vou te devolver, não preciso envolver ninguém nesse processo.

[02:31] Agora, o que você está me pedindo aqui parece ser uma rota, uma URL, que precisa ser executada por uma aplicação, em PHP, Python ou Java.

[02:41] Então o que ele faz é poder transferir a responsabilidade para algum servidor de aplicação. Esse servidor de aplicação pode estar, por exemplo, aberto ouvindo uma outra porta.

[02:54] E esse servidor web manda uma outra requisição (não utilizando o protocolo HTTP, necessariamente, pode ser usando outro formato), mas ele manda esse pedido para o servidor de aplicação.

[03:04] O servidor de aplicação processa, devolve para o servidor web, e o servidor web vai devolver para o cliente o que ele recebeu. Essa comunicação entre um servidor web e um servidor de aplicação pode ser feita de diversas formas. Então vamos focar, pelo menos no início, só nessa tarefa individual do servidor web tratando o que ele tem que tratar, de forma isolada.

[03:28] Agora, caso você já seja uma pessoa que desenvolve sistemas e que você já conhece alguma linguagem, você já sabe como fazer um servidor de aplicação funcionar.

[03:38] Então, por exemplo: com Java você pode ter um Tomcat rodando, ou com PHP você vai ter um PHP FPM rodando... Enfim. Você vai ter algum servidor de aplicação aqui, e o NGINX consegue se conectar à ele. Mas nós vamos focar só na parte do NGINX, sem nos conectarmos a algum servidor de aplicação.

[03:55] O NGINX é um servidor web, ele ouve uma porta para responder requisições HTTP - mas já existiam outros servidores que fazem exatamente isso. Acredito que o mais famoso seja o Apache, muito provavelmente você já ouviu falar do servidor HTTP da Apache.

[04:11] Qual é a grande diferença do NGINX? Simplificando também, a ideia do Apache era: você tem um servidor do Apache rodando processo do Apache. Quando chega uma requisição, esse processo cria um novo processo e trata essa requisição, depois mata esse processo e fica tudo certo.

[04:32] Só que isso é muito custoso. Criar um processo, fazer a mudança de contexto de um processo para o outro e depois matar um processo. Isso é uma tarefa custosa. Então, o que o NGINX faz?

[04:43] Ele não usa somente essa ideia de processos, threads e programação paralela, ele usa um outro conceito de programação assíncrona muito interessante - que eu vou simplificar aqui também para você entender o conceito por alto.

[04:56] Então, temos aqui uma base do funcionamento do NGINX. Quando você inicia um serviço do NGINX, ele cria um processo principal como se fosse o patrão desses colaboradores aqui.

[05:12] Então esse patrão vai criar alguns processos colaboradores, alguns worker process. Esses worker process são criados baseado no número de núcleos que o seu processador tem.

[05:29] Suponha que eu tenho um servidor que tem um processador com quatro núcleos. Então eu poderia criar, por exemplo, 4 processos worker process para tratar requisições. Porque assim eu tenho um maior número de processos tratando cada número de requisições, então eu consigo tratar mais requisições.

[05:49] Só que a sacada é: não é como se cada processo tratasse uma requisição. Não é isso! Cada um desses processos trata um número grande de requisições, várias requisições, e para que ele consiga tratar mais de um requisição, ele usa um conceito de Multiplexing I/O. Ou seja, ele faz mais de uma coisa de forma assíncrona.

[06:13] Então imagine que chegou uma requisição nesse worker aqui e depois chega uma nova requisição que caiu nesse mesmo worker. O que ele vai fazer?

[06:21] Ele vai colocar para executar o primeiro. Enquanto esse processo está esperando essa tarefa ser executada - por exemplo: o servidor de aplicação responder e o arquivo ser carregado - ele já trata outra requisição, coloca para carregar imagem e manda requisição para servidor de imagem.

[06:38] Depois ele pega a resposta da primeira requisição e devolve, continua tratando a segunda e devolve. Por isso o NGINX veio com a proposta de ser o servidor web mais rápido.

[06:49] Existem benchmarks que colocam realmente o NGINX lá no topo, ou seja, é um servidor muito performático. Mas obviamente não existe bala de prata e ele tem seus cenários onde ele trabalha muito bem e não é ferramenta ideal.

[07:04] Vamos entender melhor como ele funciona durante esse treinamento, mas a base da funcionalidade dele é essa. Ele inicia um servidor, um processo. Esse processo cria outros processos colaboradores, e cada um desses processos consegue tratar várias requisições.

[07:21] Utilizando esse conceito de programação assíncrona, ele consegue ter uma grande performance. Eu falei bastante da teoria, só que agora nós vamos partir para a prática. Vamos finalmente instalar o NGINX na nossa máquina e ver o que ele pode fazer por nós, no próximo vídeo!