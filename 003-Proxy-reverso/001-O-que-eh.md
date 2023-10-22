01
O que é?
PRÓXIMA ATIVIDADE

Pause
Mute
Current TimeÂ0:11
/
DurationÂ3:34
1xPlayback Rate
Open quality selector menuPicture-in-Picture
Close Theater Mode
Fullscreen
Transcrição
[00:00] Boas-vindas de volta a mais um capítulo desse treinamento, onde estamos brincando com o NGINX: um servidor web muito poderoso e que traz várias funcionalidades além de simplesmente ser um servidor web!

[00:12] Então vamos começar agora a falar sobre algumas dessas outras funcionalidades. A mais comum - e eu me arrisco a dizer uma das mais utilizadas - é o conceito de “proxy reverso”.

[00:23] A primeira vez que eu ouvi esse termo eu fiquei simplesmente acenando e sorrindo, sem entender absolutamente nada do que aquilo queria dizer, mas o conceito de proxy reverso não é complicado.

[00:35] Então eu vou tentar trazer para você a ideia por trás de um proxy reverso, antes de implementarmos ele, para que as coisas fiquem claras antes de sairmos digitando configuração.

[00:45] Então, um proxy de rede funciona, basicamente, como se fosse um túnel, um filtro, um intermediário; vamos chamar assim. Então imagine que você está em uma rede corporativa da sua empresa e algumas coisas são bloqueadas nessa rede, você não consegue acessar sites de notícias, site de conteúdo adulto, rede social etc.

[01:06] Isso pode ser feito através de um proxy na rede. Um proxy, basicamente, é um servidor em que todas as requisições daquela rede, antes de irem para internet, passam por esse proxy.

[01:18] Então, você no seu computador do trabalho faz uma requisição para “google.com”. Ao invés de ir diretamente para o servidor do Google, isso passa pelo servidor de proxy (pelas configurações da rede) e desse servidor de proxy, vai para o Google - ou dependendo das regras, ele trava essa requisição ali.

[01:35] Então isso é um proxy de rede, super simplificando. Um proxy reverso é quando, ao invés dessa tarefa estar no lado do cliente, nós temos algo semelhante no lado do servidor. Então essa imagem ilustra bem o que é um proxy reverso.

[01:49] Nós temos os clientes fazendo as requisições. Então, acessando a internet eles vão fazer uma requisição e ao invés de cair no servidor real, no servidor de aplicação ou servidor web, isso vai cair no proxy reverso, vai cair em um servidor web que está na frente de outros servidores web. Porque esse proxy reverso pode fazer algumas tarefas.

[02:11] Ele pode simplesmente receber todas as requisições e mandar para vários outros servidores, sem regra nenhuma. Mas ele pode realizar algumas tarefas, por exemplo, verificar cache, fazer redirecionamento, balanço de carga etc.

[02:25] Então vamos começar pela forma mais rudimentar, simplesmente redirecionando requisições. Eu recebi uma requisição aqui e eu redireciono para outro servidor. Essa é a ideia por trás de um proxy reverso.

[02:39] Quando nós entendemos esse conceito de primeira, ele não parece ser muito útil. Então de início vamos configurá-lo para não ser muito útil mesmo, vamos configurar ele para ser meio bobo, só para entendermos a configuração; depois vamos ver um caso de uso real para isso.

[02:55] Então, resumindo essa teoria por trás de um proxy reverso, é um servidor web que recebe requisições e manda para outros servidores, simples assim. Algumas regras podem acontecer aqui, caso nenhuma regra aconteça, ele não é tão útil assim. Mas quando começamos a implementar regras, um proxy reverso pode ser muito poderoso.

[03:18] Vamos começar no próximo vídeo a configurar um proxy reverso usando NGINX, ou seja, além de servidor web, um NGINX pode servir de proxy reverso. Falei bastante, vamos para a prática no próximo vídeo!