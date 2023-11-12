
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# Git

git status
git add .
git commit -m "Módulo 5 - Load Balancer. Aula 04 Formato de logs."
eval $(ssh-agent -s)
ssh-add /home/fernando/.ssh/chave-debian10-github
git push
git status



# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# 04 Formato de logs

## Transcrição
[00:00] Nós já vimos como adicionar logs. Esse é o primeiro passo, mas vamos mexer no formato desses logs, porque tem muitas coisas que nós nem precisamos complicar a visualização.

[00:12] Então vamos acessar duas coisas. Primeiro aqui no nosso ‘nginx.conf’ vamos nesse log_format main.

[00:22] Precisamos do endereço, mas não temos acesso ao usuário remoto, isso aqui vai vir sempre como um traço vazio, sem nada. Então vamos remover $remote_user. Vamos manter o horário, acho que é interessante manter.

[00:34] Aqui em “$request” é como foi a requisição, então isso eu acho legal manter. O número de bytes enviado eu acho que não tem muita necessidade, então posso remover esse $body_bytes_sent.

[00:47] Esse “$http_referer” nós vamos manter também. O “$http_user_agent” nós não precisamos, esse “$http_x_forwarded_for” não precisamos. Então posso remover toda essa linha e adicionar no final um ponto e vírgula.

[00:57] Agora vamos deixar isso um pouco mais claro. Vamos botar aqui remote addr: $remote_addr, Time: [$time_local], Request: $request”;, Status: $status, Referer: “$http_referer”;. Só para entendermos o que é o quê. Eu salvei.

[01:29] Agora nos nossos microsserviços, eu preciso usar aquele log format. Então como o nome daquele formato é ‘main’, eu vou adicionar depois do arquivo o nome do formato, que é ‘main’ - access_log /Users/vinicius.dias/Dev/nginx/logs/servico1.log main;.

[01:40] Então eu posso ter formatos diferentes para logs diferentes. No nosso caso, eu vou manter um só. Vamos garantir que eu não fiz nenhuma besteira, com o comando nginx -t. Aparentemente está tudo certo. Vamos fazer o reload com o comando nginx -s reload.

[01:51] E agora, deixe-me vir aqui no nosso tail -f e apertar a tecla “Enter” algumas vezes para limpar a tela. Quando eu acessar, eu espero ver o log naquele formato. Então vamos ver no “Serviço 1”.

[02:02] Tenho aqui. O endereço remoto é, como sempre, o da minha máquina local. Tenho aqui o horário, tem os dados do ‘request’ e tenho aqui o nosso status ‘304’, ou seja, um redirecionamento. E o ‘referer’ está vindo vazio também, então posso remover isso daqui.

[02:20] Vamos para ‘nginx.conf’ e remover para mantermos isso bem limpo. Vamos remover o ‘Referer: “$http_referer”‘. OK, removido.

[02:34] Sendo assim, nós temos um log bem mais enxuto. Tudo parece certo, vamos fazer o reload. Vamos carregar de novo o “Serviço 1”. Deixe-me acessar o “Serviço 2”. E tenho aqui bem mais enxuto.

[02:46] Então o que eu fiz aqui foi adicionar log e mexer no formato dele. Para quê eu fiz isso? Para nós conseguirmos enxergar melhor e agora, de novo, eu vou falar qual é o problema que está acontecendo.

[02:56] Eu acesso esse serviço, “http://localhost:8003”. Esse serviço acessa “http://localhost:8002” ou “http://localhost:8001”.

[03:04] Então, aqui no log do “Serviço 2” e no log do “Serviço 1”,se realmente tivéssemos máquinas diferentes, esse “Remote Addr”, ou seja, esse endereço remoto de quem está acessando o meu serviço iria ser sempre do load balancer.

[03:18] E com isso, nós não poderíamos ter estatísticas para garantirmos que estamos recebendo um ataque realmente, quais usuários realmente estão acessando a nossa aplicação - nós não iríamos ter essa informação tão preciosa.

[03:29] Então agora que nós configuramos log direito, mudamos o formato dele e estamos entendendo o problema, finalmente vamos aprender a corrigir no próximo vídeo.







# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# 04 Formato de logs

- Vamos editar nosso log_format


[00:12] Então vamos acessar duas coisas. Primeiro aqui no nosso ‘nginx.conf’ vamos nesse log_format main.

[00:22] Precisamos do endereço, mas não temos acesso ao usuário remoto, isso aqui vai vir sempre como um traço vazio, sem nada. Então vamos remover $remote_user. Vamos manter o horário, acho que é interessante manter.



- Editado:

/home/fernando/cursos/nginx-alura/005-Load-Balancer/outros/nginx.conf

adicionando informações para identificar de forma fácil
adicionando nome main 

~~~~conf
        #access_log /var/log/nginx/access.log;
        error_log /var/log/nginx/error.log;

        log_format main 'Remote Addr: $remote_addr, Time: [$time_local], Request: "$request"'
                                'Status: $status, Referer:  "$http_referer" ';
~~~~



- Editando conf do microservicos, adicionando o nome main para os logs:

/home/fernando/cursos/nginx-alura/005-Load-Balancer/sites/microservicos.conf

~~~~conf
server {
        listen 8001;
        server_name localhost;

        access_log /var/log/nginx/servico1.log;

        location / {
            root /home/fernando/cursos/nginx-alura/005-Load-Balancer/arquivos/servico1 main;
            index index.html;
        }

}

server {
        listen 8002;
        server_name localhost;

        access_log /var/log/nginx/servico2.log;

        location / {
            root /home/fernando/cursos/nginx-alura/005-Load-Balancer/arquivos/servico2 main;
            index index.html;
        }

}
~~~~




- Validando.
- ERRO:

~~~~BASH

root@debian10x64:/etc/nginx# nginx -t
nginx: [emerg] invalid number of arguments in "root" directive in /etc/nginx/sites-enabled/microservicos.conf:8
nginx: configuration file /etc/nginx/nginx.conf test failed
root@debian10x64:/etc/nginx#

~~~~







- Ajustado:

main estava no local errado
movimentado para a linha do access_log

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



- Validando.
- OK agora:

~~~~BASH

root@debian10x64:/etc/nginx# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx# nginx -s reload
root@debian10x64:/etc/nginx#

~~~~









root@debian10x64:/etc/nginx/sites-enabled# curl http://192.168.0.110:8003/
Serviço 1
root@debian10x64:/etc/nginx/sites-enabled#

root@debian10x64:/home/fernando# tail -f /var/log/nginx/servico1.log
127.0.0.1 - - [12/Nov/2023:18:09:11 -0300] "GET / HTTP/1.0" 200 11 "-" "curl/7.64.0"
Remote Addr: 127.0.0.1, Time: [12/Nov/2023:19:46:26 -0300], Request: "GET / HTTP/1.0"Status: 200, Referer:  "-"


root@debian10x64:/etc/nginx/sites-enabled# curl http://192.168.0.110:8003/
Serviço 2
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled#



root@debian10x64:/home/fernando# tail -f /var/log/nginx/servico2.log
127.0.0.1 - - [12/Nov/2023:18:10:16 -0300] "GET / HTTP/1.0" 200 11 "-" "curl/7.64.0"
Remote Addr: 127.0.0.1, Time: [12/Nov/2023:19:46:54 -0300], Request: "GET / HTTP/1.0"Status: 200, Referer:  "-"

