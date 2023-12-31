
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





# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# 02 Definindo pesos

- Configuração atual do nosso Load Balancer no NGINX:

~~~~CONF
upstream servicos {
        server localhost:8001;
        server localhost:8002;
}

server {
        listen 8003;
        server_name localhost;

        location / {
            proxy_pass http://servicos;
            proxy_set_header X-Real-IP $remote_addr;
        }
}
~~~~





## Weighted load balancing

It is also possible to influence nginx load balancing algorithms even further by using server weights.

In the examples above, the server weights are not configured which means that all specified servers are treated as equally qualified for a particular load balancing method.

With the round-robin in particular it also means a more or less equal distribution of requests across the servers — provided there are enough requests, and when the requests are processed in a uniform manner and completed fast enough.

When the weight parameter is specified for a server, the weight is accounted as part of the load balancing decision.

        upstream myapp1 {
            server srv1.example.com weight=3;
            server srv2.example.com;
            server srv3.example.com;
        }

With this configuration, every 5 new requests will be distributed across the application instances as the following: 3 requests will be directed to srv1, one request will go to srv2, and another one — to srv3.

It is similarly possible to use weights with the least-connected and ip-hash load balancing in the recent versions of nginx. 









- Configuração onde o nosso Load Balancer no NGINX vai efetuar mais requisições para o servidor1, já que tem o peso maior.
- Já o serviço2 vai ficar com o peso default, que é o peso 1.
- Configuração ajustada:

~~~~CONF
upstream servicos {
        server localhost:8001 weight=3;
        server localhost:8002;
}

server {
        listen 8003;
        server_name localhost;

        location / {
            proxy_pass http://servicos;
            proxy_set_header X-Real-IP $remote_addr;
        }
}
~~~~




- Ajustando:

~~~~bash

root@debian10x64:/etc/nginx# vi sites-enabled/load-balancer.conf
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx# nginx -s reload
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx# cat sites-enabled/load-balancer.conf
upstream servicos {
        server localhost:8001 weight=3;
        server localhost:8002;
}

server {
        listen 8003;
        server_name localhost;

        location / {
            proxy_pass http://servicos;
            proxy_set_header X-Real-IP $remote_addr;
        }
}
root@debian10x64:/etc/nginx#

~~~~




- Deu erro 404:

~~~~bash
root@debian10x64:/etc/nginx# curl http://192.168.0.110:8003/
<html>
<head><title>404 Not Found</title></head>
<body bgcolor="white">
<center><h1>404 Not Found</h1></center>
<hr><center>nginx/1.14.2</center>
</body>
</html>
root@debian10x64:/etc/nginx#
~~~~

verificando




- Subindo o serviço do PHP

~~~~bash
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx# php -S localhost:8000
[Sat Dec  2 21:08:20 2023] PHP 8.0.30 Development Server (http://localhost:8000) started
~~~~


- Mesmo assim continua o erro 404:

~~~~bash
root@debian10x64:/etc/nginx# curl http://192.168.0.110:8003/
<html>
<head><title>404 Not Found</title></head>
<body bgcolor="white">
<center><h1>404 Not Found</h1></center>
<hr><center>nginx/1.14.2</center>
</body>
</html>
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx# date
Sat 02 Dec 2023 09:09:45 PM -03
root@debian10x64:/etc/nginx#
~~~~



- Verificando logs do NGINX

~~~~bash
root@debian10x64:/var/log/nginx# ls -ltr
total 132
[...]]
-rw-r----- 1 www-data adm  1763 Nov 12 20:13 access.log.1
-rw-r----- 1 www-data adm   103 Dec  2 21:06 servico2.log
-rw-r----- 1 www-data adm   309 Dec  2 21:08 servico1.log
-rw-r----- 1 www-data adm  1583 Dec  2 21:08 error.log
-rw-r----- 1 www-data adm   360 Dec  2 21:08 access.log
root@debian10x64:/var/log/nginx#
root@debian10x64:/var/log/nginx#
root@debian10x64:/var/log/nginx# date
Sat 02 Dec 2023 09:09:16 PM -03
root@debian10x64:/var/log/nginx#

root@debian10x64:/var/log/nginx# tail error.log
2023/12/02 21:04:46 [notice] 77488#77488: signal process started
2023/12/02 21:05:26 [error] 77490#77490: *121 "/home/fernando/cursos/nginx-alura/005-Load-Balancer/arquivos/servico1/index.html" is not found (2: No such file or directory), client: 127.0.0.1, server: localhost, request: "GET / HTTP/1.0", host: "servicos"
2023/12/02 21:06:14 [error] 77489#77489: *122 connect() failed (111: Connection refused) while connecting to upstream, client: 192.168.0.110, server: localhost, request: "GET / HTTP/1.1", upstream: "http://[::1]:8001/", host: "192.168.0.110:8003"
2023/12/02 21:06:14 [error] 77489#77489: *125 "/home/fernando/cursos/nginx-alura/005-Load-Balancer/arquivos/servico2/index.html" is not found (2: No such file or directory), client: 127.0.0.1, server: localhost, request: "GET / HTTP/1.0", host: "servicos"
2023/12/02 21:08:28 [error] 77489#77489: *128 "/home/fernando/cursos/nginx-alura/005-Load-Balancer/arquivos/servico1/index.html" is not found (2: No such file or directory), client: 127.0.0.1, server: localhost, request: "GET / HTTP/1.0", host: "servicos"
2023/12/02 21:08:30 [error] 77489#77489: *129 connect() failed (111: Connection refused) while connecting to upstream, client: 192.168.0.110, server: localhost, request: "GET / HTTP/1.1", upstream: "http://[::1]:8002/", host: "192.168.0.110:8003"
2023/12/02 21:08:30 [error] 77489#77489: *132 "/home/fernando/cursos/nginx-alura/005-Load-Balancer/arquivos/servico1/index.html" is not found (2: No such file or directory), client: 127.0.0.1, server: localhost, request: "GET / HTTP/1.0", host: "servicos"
2023/12/02 21:09:44 [error] 77489#77489: *133 connect() failed (111: Connection refused) while connecting to upstream, client: 192.168.0.110, server: localhost, request: "GET / HTTP/1.1", upstream: "http://[::1]:8001/", host: "192.168.0.110:8003"
2023/12/02 21:09:44 [error] 77490#77490: *136 "/home/fernando/cursos/nginx-alura/005-Load-Balancer/arquivos/servico1/index.html" is not found (2: No such file or directory), client: 127.0.0.1, server: localhost, request: "GET / HTTP/1.0", host: "servicos"
root@debian10x64:/var/log/nginx#
root@debian10x64:/var/log/nginx#
root@debian10x64:/var/log/nginx#
~~~~



- Como foi alterada a estrutura de pastas do projeto no meu Github, acabou trazendo este problema.

- Copiando arquivos HTML dos Microserviços para nova pasta:

~~~~bash
root@debian10x64:/etc/nginx# mkdir /var/www/site-aula-nginx-alura
root@debian10x64:/etc/nginx#

root@debian10x64:/etc/nginx# cp -R /home/fernando/cursos/nginx-alura/NGINX-Parte-1/005-Load-Balancer/arquivos/servico1 /var/www/site-aula-nginx-alura
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx# ls /var/www/site-aula-nginx-alura
servico1
root@debian10x64:/etc/nginx# cp -R /home/fernando/cursos/nginx-alura/NGINX-Parte-1/005-Load-Balancer/arquivos/servico2 /var/www/site-aula-nginx-alura
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx# ls /var/www/site-aula-nginx-alura
servico1  servico2
root@debian10x64:/etc/nginx#
~~~~



- Ajustando conf do NGINX referente aos Microserviços:

- ANTES

~~~~conf
server {
        listen 8001;
        server_name localhost;

        access_log /var/log/nginx/servico1.log main;

        location / {
            root /home/fernando/cursos/nginx-alura/005-Load-Balancer/arquivos/servico1;
            index index.html;
        }

}

server {
        listen 8002;
        server_name localhost;

        access_log /var/log/nginx/servico2.log main;

        location / {
            root /home/fernando/cursos/nginx-alura/005-Load-Balancer/arquivos/servico2;
            index index.html;
        }

}
~~~~



- DEPOIS:

~~~~conf
server {
        listen 8001;
        server_name localhost;

        access_log /var/log/nginx/servico1.log main;

        location / {
            root /var/www/site-aula-nginx-alura/servico1;
            index index.html;
        }

}

server {
        listen 8002;
        server_name localhost;

        access_log /var/log/nginx/servico2.log main;

        location / {
            root /var/www/site-aula-nginx-alura/servico2;
            index index.html;
        }

}
~~~~




- Ajustando

~~~~bash
root@debian10x64:/etc/nginx# vi sites-enabled/microservicos.conf
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx# nginx -s reload
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx#
~~~~




- Novo teste
curl http://192.168.0.110:8003/

~~~~bash

root@debian10x64:/etc/nginx# curl http://192.168.0.110:8003/
Serviço 1
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx# curl http://192.168.0.110:8003/
Serviço 2
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx# curl http://192.168.0.110:8003/
Serviço 1
root@debian10x64:/etc/nginx# curl http://192.168.0.110:8003/
Serviço 1
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx# curl http://192.168.0.110:8003/
Serviço 1
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx# curl http://192.168.0.110:8003/
Serviço 1
root@debian10x64:/etc/nginx# curl http://192.168.0.110:8003/
Serviço 2
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx# curl http://192.168.0.110:8003/
Serviço 1
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx# curl http://192.168.0.110:8003/
Serviço 1
root@debian10x64:/etc/nginx# curl http://192.168.0.110:8003/
Serviço 1
root@debian10x64:/etc/nginx# curl http://192.168.0.110:8003/
Serviço 2
root@debian10x64:/etc/nginx# curl http://192.168.0.110:8003/
Serviço 1
root@debian10x64:/etc/nginx#

~~~~


Balanceamento ocorreu conforme o esperado, respeitando o Weight.


- Configuração atual do Load Balancer:

~~~~conf
upstream servicos {
        server localhost:8001 weight=3;
        server localhost:8002;
}

server {
        listen 8003;
        server_name localhost;

        location / {
            proxy_pass http://servicos;
            proxy_set_header X-Real-IP $remote_addr;
        }
}

~~~~