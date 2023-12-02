
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# Git

git status
git add .
git commit -m "Módulo 1 - Load Balancer. Aula 02 Definindo pesos."
eval $(ssh-agent -s)
ssh-add /home/fernando/.ssh/chave-debian10-github
git push
git status



# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# 02 Definindo pesos

## Transcrição

[00:00] No final do último treinamento, o que nós fizemos? Nós implementamos essa ilustração, um load balancer, ou um balanceador de cargas. Vamos recapitular o que fizemos, como isso foi implementado para pensarmos em um possível problema.

[00:18] Temos um cliente, você, eu, qualquer pessoa na internet, acessando algum site, imagino que é o site da Alura. E o site da Alura por receber muitas requisições precisamos separar em mais de um servidor. Caso um servidor caia temos outro de pé, funcionando, para não sobrecarregar esses servidores, porque eles não aguentariam toda essa carga. Por vários motivos precisamos dividir esses servidores.

[00:45] Quando esse cliente acessa um site da Alura temos o engine next configurado como um load balancer e cada requisição vai cair em um servidor diferente. Chegou a primeira requisição? Cai no primeiro. Chego outra requisição? Cai no segundo. Chegou mais uma? Primeiro, segundo, e vai dividindo igualmente as requisições.

[01:04] Se temos 100 requisições chegando, 50 vão chegar para um servidor, 50 vão chegar para o outro. Vamos só recapitular rápido como implementamos isso. Vou abrir meu terminal. Lembrando que como estou no Mac meus caminhos são esses, mas para você saber o caminho na sua configuração, no seu sistema operacional, nginx -t vai mostrar o arquivo padrão.

[01:25] Então meu arquivo padrão é esse nginx conf, nesse arquivo padrão sei que ele inclui todos os arquivos .conf de servers. Temos um load balancer, vamos abrir ele para ver e se precisar já modificar. Então usr/local/etc/nginx/servers/load-balancer, nesse load balancer estamos ouvindo a porta 8003 e cada hora que chega uma requisição para essa porta hora ele vai mandar para o serviço 1 hora ele vai mandar para o serviço 2.

[02:00] Vamos testar isso de novo só para relembrarmos, hora ele manda para o serviço 1, e quando atualizo ele vai para o serviço 2. Ele vai alternando entre um e outro.

[02:22] Recapitulamos o conceito de load balancer, vamos avançar um pouco. Imagine que por algum motivo, porque nossa infraestrutura é assim, porque isso foi configurado dessa forma, por algum motivo temos um servidor maior do que outro, com mais capacidade de responder requisições.

[02:38] Imagine que nós temos dois servidores respondendo, no nosso caso o de serviço 1 e o de serviço 2. E o servidor que contém esse serviço 1, que está respondendo nessa porta, tem o dobro de memória RAM, de CPU, o dobro da capacidade de receber requisições. Então se ficamos mandando metade para cada um, se mantemos essa estrutura de chega uma requisição vai para um, chega outro vai para outro, vamos acabar sobrecarregando esse servidor menor ou não utilizando todos os recursos disponíveis no servidor maior.

[03:12] O que quero fazer é poder atribuir pesos diferentes para cada um dos meus servidores disponíveis, e fazer isso com nginx é muito simples, posso informar que este servidor 1 tem o peso de 2. Posso usar qualquer número, posso dizer que tem peso 5 enquanto o outro tem peso 2, ou seja, a cada 7 requisições que chegam no meu sistema 5 vão cair no serviço 1 e duas vão cair no serviço 2.

[03:42] Posso configurar esse peso de forma proporcional. Vou manter configurado o serviço 1 com peso 2 e o serviço 2 vai estar no padrão dele, que é o peso 1. Vou salvar isso, mas lembrando, se alterei essa configuração preciso fazer o meu reload. Nginx recarregado sem erro nenhum, vamos ver o que vai acontecer quando faço as requisições.

[04:10] Caiu uma requisição no serviço 1, recarreguei mais uma requisição no serviço 1, recarreguei agora no serviço 2. Quando recarrego de novo, serviço 1, serviço 1, serviço 2. A cada duas requisições no serviço 1, uma requisição cai no serviço 2. Dessa forma conseguimos balancear melhor nossa carga. E esse é um dos algoritmos possíveis para balancear a carga. Então vamos falar um pouco de teoria já que já vimos a prática no próximo vídeo.