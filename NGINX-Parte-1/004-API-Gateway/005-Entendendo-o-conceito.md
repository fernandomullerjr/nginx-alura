
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# Git

git status
git add .
git commit -m "Módulo 4 - API Gateway. Aula 05 Entendendo o conceito."
eval $(ssh-agent -s)
ssh-add /home/fernando/.ssh/chave-debian10-github
git push
git status



# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# 05 Entendendo o conceito

## Transcrição
[00:00] Vamos recapitular rapidamente o que nós fizemos no último vídeo, porque foi um pouco complexo, apesar de todos os conceitos que já vimos no capítulo anterior.

[00:12] O que nós fizemos foi ter no servidor que no meu computador está ouvindo a porta 8080. Nós temos dois proxy, ou seja, proxy reverso para dois serviços diferentes.

[00:24] Sempre que eu acessar ‘/servico1 algumacoisa’, esse ‘algumacoisa’ vai ser enviado para o serviço que está rodando na porta 8001. Se eu acessar ‘/servico2 algumacoisa’ esse ‘algumacoisa’ vai ser enviado para o ‘localhost:8002’.

[00:41] Então, dessa forma nós construímos um proxy reverso para dois serviços diferentes - e é muito comum que tenhamos serviços diferentes realizando tarefas diferentes. Principalmente quando utilizamos uma arquitetura orientada a microsserviços.

[00:55] Então o que nós fizemos aqui foi criar serviços diferentes e ter um ponto de entrada através desse servidor para esses serviços - e esse ponto de entrada tem um nome.

[01:07] Em uma arquitetura de microsserviços, quando nós temos um ponto único de entrada, chamamos isso de API Gateway ou de portão de uma API. Então, a partir de uma API Gateway é decidido para onde essa requisição vai ser realmente redirecionada.

[01:23] Então, o problema que essa técnica visa resolver é aquela ideia de clientes acessando livremente cada um dos serviços e de que às vezes um serviço ou domínio muda para outro servidor, ou alguma coisa assim do tipo.

[01:36] E eu não precisaria notificar meu cliente, meu cliente não precisaria saber dessa complexidade, mas ele precisaria se atualizar. Enfim, ele tem acesso demais à minha infraestrutura.

[01:48] Para o cliente, para quem está acessando a nossa API e que usa microsserviços, isso deveria ser transparente. Então nós expomos uma única URL, um único servidor, que é essa nossa API Gateway; e a partir desse servidor (“api.alura.com.br”, por exemplo) ele vai fazer todas as requisições, mandando o endpoint correto e cada uma das requisições vai ser redirecionada para o serviço certo.

[02:14] Então o API Gateway fornece um proxy, como temos utilizado o proxy reverso, para as necessidades reais.

[02:22] Uma desvantagem interessante de falar é que esse portão de entrada pode se tornar um ponto único central de falha. E o que isso quer dizer? Se esse meu servidor, que só redireciona, estiver recebendo muitas requisições, nós podemos ter um downtime, podemos ter um atraso, nós podemos ter uma falha ali naquele ponto e derrubarmos toda a aplicação.

[02:47] Então esse é um ponto crítico que devemos monitorar com bastante cuidado - mas vamos falar um pouco sobre performance ainda, então não vamos entrar tanto nesse detalhe.

[02:57] E o comportamento de um API Gateway é importante conhecermos porque ele pode simplesmente redirecionar as requisições e pode permitir ou não uma requisição, então através do NGINX eu poderia configurar para só deixar as requisições vindas de um IP, por exemplo. Isso seria possível.

[03:16] Eu posso utilizar algum tipo de decorator para adicionar informações necessárias no request ou até para remover da resposta. Eu posso adicionar informações de cache nesse meu API Gateway e eu posso adicionar informações de compressão para melhorar performance. Então eu posso adicionar cabeçalhos na ida e eu posso adicionar cabeçalhos na resposta.

[03:38] Então eu posso modificar esses detalhes e eu também posso acabar limitando o acesso. Assim como eu falei de autorizar ou não, eu posso utilizar essa ideia de API Gateway para limitar o acesso, o conteúdo trafegado e impedir que determinadas URLs sejam acessadas por completo. Então eu posso ter um controle a mais.

[03:59] Claro que, para determinados comportamentos e determinadas configurações, não adianta eu ter somente um proxy reverso. Eu precisaria ter uma aplicação ali no meu API Gateway que redirecionasse as chamadas. Mas para a nossa necessidade, onde eu só preciso redirecionar as chamadas, o NGINX já traz isso pronto, sem precisar de nenhum outro software externo.

[04:20] Então essa é a ideia por trás de um API Gateway usando o NGINX!

[04:24] Eu vou deixar na atividade “Para Saber Mais” uma referência muito interessante com um pouco mais de detalhes, do próprio site do NGINX, mostrando como implementar esse API Gateway.

[04:34] Mas apesar de deixar esse “Para Saber Mais” aqui, eu vou pedir para você ler depois do final desse treinamento. Porque tem mais um conceito que vamos aprender que é utilizado naquele artigo.

[04:44] Então, como eu falei um pouco aqui nessa parte de negrito de ponto central de falha, de receber muitas requisições, eu acho que está na hora de pelo menos começarmos a dar uma introdução ao conceito de performance.

[04:58] O que acontece se um servidor meu de aplicação real que realmente responde as requisições se um servidor meu, não o API Gateway, estiver recebendo muitas requisições a mais do que ele consegue responder? O que nós podemos fazer nesses cenários? Como nós podemos, por exemplo, ter mais de um servidor respondendo a mesma aplicação?

[05:20] Então vamos conversar sobre esses cenários no próximo capítulo!









# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# RESUMO


[02:57] E o comportamento de um API Gateway é importante conhecermos porque ele pode simplesmente redirecionar as requisições e pode permitir ou não uma requisição, então através do NGINX eu poderia configurar para só deixar as requisições vindas de um IP, por exemplo. Isso seria possível.

[03:16] Eu posso utilizar algum tipo de decorator para adicionar informações necessárias no request ou até para remover da resposta. Eu posso adicionar informações de cache nesse meu API Gateway e eu posso adicionar informações de compressão para melhorar performance. Então eu posso adicionar cabeçalhos na ida e eu posso adicionar cabeçalhos na resposta.

[03:38] Então eu posso modificar esses detalhes e eu também posso acabar limitando o acesso. Assim como eu falei de autorizar ou não, eu posso utilizar essa ideia de API Gateway para limitar o acesso, o conteúdo trafegado e impedir que determinadas URLs sejam acessadas por completo. Então eu posso ter um controle a mais.


