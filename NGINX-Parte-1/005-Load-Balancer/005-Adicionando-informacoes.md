
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# Git

git status
git add .
git commit -m "Módulo 5 - Load Balancer. Aula 05 Adicionando informações."
eval $(ssh-agent -s)
ssh-add /home/fernando/.ssh/chave-debian10-github
git push
git status



# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# 05 Adicionando informações

## Transcrição
[00:00] Finalmente vamos resolver o problema que eu disse que iríamos resolver.

[00:06] O que eu quero é que sempre que eu receba uma requisição naquele nosso load balancer, além de fazer o proxy_pass, eu quero adicionar um cabeçalho HTTP. Então deixe-me abrir aqui o load-balancer.conf.

[00:23] O que eu posso fazer é adicionar aqui uma nova diretiva proxy_set_header. Quando eu disse que o load balancer, um API Gateway ou que algum proxy reverso qualquer podem fazer manipulações, é disso que eu estava falando. Nós podemos adicionar, por exemplo, um novo cabeçalho.

[00:44] Eu vou adicionar um cabeçalho personalizado, que eu posso dar o nome que eu quiser. Segundo a especificação HTTP, quando vamos adicionar cabeçalhos personalizados que não fazem da especificação, o ideal é começarmos com ‘X-’ e o que vier depois nós escreveremos o que quisermos.

[01:00] Então eu vou chamar isso de proxy_set_header X-Real-IP, mas eu posso chamar do que eu quiser, ‘IP-de-verdade’, só ‘IP’, ‘endereco’ - posso chamar como eu quiser.

[01:13] E aqui o valor que isso vai ter é o $remote_addr;. Então eu sei que essa variável existe porque ela está lá no log format. Então existem diversos valores que nós podemos pegar aqui, como já vimos no log format.

[01:28] Então eu estou pegando o endereço que está mandando a requisição para o meu load balancer e adicionando em um cabeçalho que vai ser enviado para os meus serviços. Essa é a primeira etapa.

[01:39] Agora nos serviços eu posso adicionar naquele formato do log. Invés de utilizar o $remote_addr, eu posso utilizar esse cabeçalho, que é ‘X-Real-IP’. Vamos ver como podemos fazer isso. O nosso formato de log está aqui no arquivo principal de configuração.

[02:01] Então ao invés de usar o $remote_addr, eu vou utilizar $http_x_real_ip, trocando o traço por underline eu posso pegar qualquer cabeçalho.

[02:17] Então dessa forma, imagine que o meu IP seja ‘1921687’. Eu fiz requisição para esse serviço, que é '1921683’. Esse serviço fez o proxy reverso e mandou requisição para ‘1921682’.

[02:34] Então no log desse “Serviço 2”, o IP que vai estar como IP remoto agora vai ser o meu ‘1921687’, o IP da minha máquina. E assim, eu vou ter acesso aos IPs reais.

[02:46] Então vamos garantir que eu não fiz nenhuma bobagem. Eu vou usar o comando ‘nginx -s reload’ para garantir que o resultado continue sendo o mesmo. Deixe-me quebrar a linha aqui, para garantir. E vamos lá acessar no navegador http://localhost:8003. Ele deve acessar o “Serviço 1”, agora o “Serviço 2”.

[03:03] Ele continua conseguindo pegar o ‘remote addr’ nos dois locais, só que com a diferença de que se eu estiver em máquinas diferentes, aqui eu vou ter realmente o IP de quem mandou a requisição.

[03:15] Isso é muito importante quando trabalhamos com load balancer, mas também quando fazemos qualquer outro tipo de proxy reverso. Seja mandando para um servidor de aplicação, e aí precisamos saber como fazer isso; seja mandando para outros servidores HTTP, somente como proxy reverso; ou nesse caso, como load balancer.

[03:33] Nós precisamos configurar esse tipo de coisa, precisamos saber o que estamos fazendo para garantirmos que teremos acesso às informações corretas.

[03:40] Como eu já disse, utilizar o proxy pass e fazer uma outra requisição HTTP não é a única forma de ter um proxy reverso. Existem outros protocolos que podemos utilizar e esses outros protocolos podem lidar com esse cenário de formas diferentes - mas isso podemos tratar no outro treinamento!




# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# 05 Adicionando informações


[00:06] O que eu quero é que sempre que eu receba uma requisição naquele nosso load balancer, além de fazer o proxy_pass, eu quero adicionar um cabeçalho HTTP. Então deixe-me abrir aqui o load-balancer.conf.

[00:23] O que eu posso fazer é adicionar aqui uma nova diretiva proxy_set_header. Quando eu disse que o load balancer, um API Gateway ou que algum proxy reverso qualquer podem fazer manipulações, é disso que eu estava falando. Nós podemos adicionar, por exemplo, um novo cabeçalho.

[00:44] Eu vou adicionar um cabeçalho personalizado, que eu posso dar o nome que eu quiser. Segundo a especificação HTTP, quando vamos adicionar cabeçalhos personalizados que não fazem da especificação, o ideal é começarmos com ‘X-’ e o que vier depois nós escreveremos o que quisermos.

[01:00] Então eu vou chamar isso de proxy_set_header X-Real-IP, mas eu posso chamar do que eu quiser, ‘IP-de-verdade’, só ‘IP’, ‘endereco’ - posso chamar como eu quiser.

[01:13] E aqui o valor que isso vai ter é o $remote_addr;. Então eu sei que essa variável existe porque ela está lá no log format. Então existem diversos valores que nós podemos pegar aqui, como já vimos no log format.

[01:28] Então eu estou pegando o endereço que está mandando a requisição para o meu load balancer e adicionando em um cabeçalho que vai ser enviado para os meus serviços. Essa é a primeira etapa.



- Ajustando a conf do Load Balancer:

/home/fernando/cursos/nginx-alura/005-Load-Balancer/sites/load-balancer.conf

ANTES

~~~~conf
upstream servicos {
        server localhost:8001;
        server localhost:8002;
}

server {
        listen 8003;
        server_name localhost;

        location / {
            proxy_pass http://servicos;
        }
}
~~~~


DEPOIS

~~~~conf
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









[01:39] Agora nos serviços eu posso adicionar naquele formato do log. Invés de utilizar o $remote_addr, eu posso utilizar esse cabeçalho, que é ‘X-Real-IP’. Vamos ver como podemos fazer isso. O nosso formato de log está aqui no arquivo principal de configuração.

[02:01] Então ao invés de usar o $remote_addr, eu vou utilizar $http_x_real_ip, trocando o traço por underline eu posso pegar qualquer cabeçalho.


DE:

~~~~CONF

        log_format main 'Remote Addr: $remote_addr, Time: [$time_local], Request: "$request"'
                                ' Status: $status ';

~~~~

PARA:

~~~~CONF

        log_format main 'Remote Addr: $http_x_real_ip, Time: [$time_local], Request: "$request"'
                                ' Status: $status ';

~~~~




- Ajustando:

~~~~bash

root@debian10x64:/etc/nginx# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx# nginx -s reload
root@debian10x64:/etc/nginx#
~~~~




- Testando:

~~~~bash

root@debian10x64:/etc/nginx/sites-enabled# curl http://192.168.0.110:8003/
Serviço 1
root@debian10x64:/etc/nginx/sites-enabled#

root@debian10x64:/home/fernando# tail -f /var/log/nginx/servico1.log
127.0.0.1 - - [12/Nov/2023:18:09:11 -0300] "GET / HTTP/1.0" 200 11 "-" "curl/7.64.0"
Remote Addr: 127.0.0.1, Time: [12/Nov/2023:19:46:26 -0300], Request: "GET / HTTP/1.0"Status: 200, Referer:  "-"
Remote Addr: 127.0.0.1, Time: [12/Nov/2023:19:51:13 -0300], Request: "GET / HTTP/1.0" Status: 200
Remote Addr: 192.168.0.110, Time: [12/Nov/2023:20:13:51 -0300], Request: "GET / HTTP/1.0" Status: 200


root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled# curl http://192.168.0.110:8003/
Serviço 2
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled#

root@debian10x64:/home/fernando# tail -f /var/log/nginx/servico2.log
127.0.0.1 - - [12/Nov/2023:18:10:16 -0300] "GET / HTTP/1.0" 200 11 "-" "curl/7.64.0"
Remote Addr: 127.0.0.1, Time: [12/Nov/2023:19:46:54 -0300], Request: "GET / HTTP/1.0"Status: 200, Referer:  "-"
Remote Addr: 127.0.0.1, Time: [12/Nov/2023:19:51:15 -0300], Request: "GET / HTTP/1.0" Status: 200
Remote Addr: 192.168.0.110, Time: [12/Nov/2023:20:13:52 -0300], Request: "GET / HTTP/1.0" Status: 200


~~~~



[03:03] Ele continua conseguindo pegar o ‘remote addr’ nos dois locais, só que com a diferença de que se eu estiver em máquinas diferentes, aqui eu vou ter realmente o IP de quem mandou a requisição.

[03:15] Isso é muito importante quando trabalhamos com load balancer, mas também quando fazemos qualquer outro tipo de proxy reverso. Seja mandando para um servidor de aplicação, e aí precisamos saber como fazer isso; seja mandando para outros servidores HTTP, somente como proxy reverso; ou nesse caso, como load balancer.

[03:33] Nós precisamos configurar esse tipo de coisa, precisamos saber o que estamos fazendo para garantirmos que teremos acesso às informações corretas.

[03:40] Como eu já disse, utilizar o proxy pass e fazer uma outra requisição HTTP não é a única forma de ter um proxy reverso. Existem outros protocolos que podemos utilizar e esses outros protocolos podem lidar com esse cenário de formas diferentes - mas isso podemos tratar no outro treinamento!