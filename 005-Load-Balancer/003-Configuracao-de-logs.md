
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# Git

git status
git add .
git commit -m "Módulo 5 - Load Balancer. Aula 01 Upstreams."
eval $(ssh-agent -s)
ssh-add /home/fernando/.ssh/chave-debian10-github
git push
git status



# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# 03 Configuração de logs

## Transcrição
[00:00] Quando trabalhamos com proxy reverso ou com o load balancer nós precisamos fazer algumas modificações no log para garantirmos que nós temos a informação correta.

[00:13] Então antes de fazermos qualquer modificação, vamos habilitar os logs para a nossa aplicação. Se dermos uma olhada no nosso arquivo de configuração padrão, nós temos dois tipos de log.

[00:23] Nós temos error_log, como estamos vendo aqui, e o acess_log. A diferença é que o acess_log é qualquer log de acesso. Sempre que uma requisição é processada, o NGINX salva um log. Já um error_log é um log somente de erros, então se aconteceu algum problema, ele vai gerar esse tipo de log.

[00:45] Então eu quero configurar somente o acess_log. Então eu preciso usar essa diretiva #access_log.

[00:53] E nós podemos passar um formato do log. Então repare que nós temos aqui um formato main desse log. Então eu vou descomentar isso daqui, ou seja, eu vou habilitar esse formato de log.

[01:08] Só que eu não vou habilitar o acess_log ainda, porque eu vou querer habilitar o acess_log para cada um dos meus servidores em arquivos diferentes. Então, vamos lá!

[01:18] Primeiramente, deixe-me eu decidir onde que eu vou salvar esses logs. Eu vou salvar aqui em mkdir Dev/nginx/logs/. Um detalhe importante é que o caminho para os nossos logs precisam ser em uma pasta que já exista, o NGINX não vai criar essa pasta, tem que ser em algum lugar que o usuário do NGINX tenha a permissão de escrita. Nós podemos definir qual o usuário do NGINX através de ‘user’.

[01:46] Mas nessa pasta que eu acabei de criar, qualquer usuário tem permissão, então vamos nessa. Eu vou acessar os meus serviços - vim /usr/local/etc/nginx/servers/microsservicos.conf.

[02:02] Então nesses dois serviços eu vou ativar o acess_log. Então, vamos lá! access_log /Users/vinicius.dias/Dev/nginx/logs/servico1.log;. Então esse vai ser o meu acess_log do meu ‘servico1’ e esse vai ser o acess_log do meu ‘servico2’ - access_log /Users/vinicius.dias/Dev/nginx/logs/servico2.log;.

[02:27] Então deixe-me garantir que usei a pasta certa. Isso aí, logs! Então deixe-me garantir que aqui está tudo certo. Deixe-me recarregar com o comando nginx -s reload. Aqui eu vou criar uma nova aba para fazer um tail -f Dev/nginx/logs/servico1.log.

[02:48] Então esse comando tail -f fica lendo esse arquivo. E sempre que acontecer alguma coisa; se alguma linha for escrita nesse arquivo, já vai aparecer diretamente aqui no comando.

[02:59] Então vou fazer a mesma coisa com o ‘servico2’ - tail -f Dev/nginx/logs/servico2.log. Então eu estou lendo os dois arquivos e agora vou utilizar aquele nosso load balancer.

[03:14] Vou acessar no navegador, http://localhost:8003.

[03:15] Primeiro caiu no “Serviço 1”, então eu espero ver aqui no terminal o log do ‘servico1’.

[03:19] Perfeito! Agora quando eu acessar de novo, eu vou cair no “Serviço 2”

[03:26] E eu tenho aqui no terminal, o log do ‘servico2’.

[03:28] E vamos bater o olho aqui rápido no formato desse log.

[03:33] O que nós temos é: primeiro, o endereço remoto - ou seja, o endereço de quem fez essa requisição; o usuário remoto; o horário local; os dados da requisição; e aí tem o status; o corpo da requisição; e esse monte de coisas sobre a requisição em si.

[03:51] Então aqui nós temos o nosso servidor que fez a requisição, que é sempre aqui da máquina local; nós temos a data e hora, ou seja, o momento; os dados da requisição em si; nós não temos nenhum corpo, mas aqui nós temos a requisição completa com o user agent e tudo. Então nós vemos que tem muitas informações aqui.

[04:13] Só que tem um detalhe muito importante. Aqui, todas as requisições estão vindo do mesmo local, porque eu que estou fazendo da minha máquina local, e eu acesso outro servidor que está na minha máquina local, faço proxy reverso para a minha máquina local. Está tudo aqui.

[04:28] Só que tem um detalhe muito importante: imagine que “localhost:8003” estivesse no IP “1921683”, aí quando eu acesso ele, ele redireciona para “1921682”. Então, aqui no log do meu ‘servico2’, ele mostraria que quem fez a requisição é o IP do nosso load balancer.

[04:54] Então isso é um problema quando trabalhamos realmente com servidores diferentes, porque nós perderíamos a informação real de quem está fazendo essa requisição.

[05:04] Então, para resolvermos esse problema, nós temos algumas possíveis soluções. Nós vamos ver como adicionar informações a esse log, como podemos enviar detalhes a mais usando proxy reverso e pegar essas informações para adicionar no log. Só que isso eu vou fazer no próximo vídeo!



# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# 03 Configuração de logs


[01:18] Primeiramente, deixe-me eu decidir onde que eu vou salvar esses logs. Eu vou salvar aqui em mkdir Dev/nginx/logs/. Um detalhe importante é que o caminho para os nossos logs precisam ser em uma pasta que já exista, o NGINX não vai criar essa pasta, tem que ser em algum lugar que o usuário do NGINX tenha a permissão de escrita. Nós podemos definir qual o usuário do NGINX através de ‘user’.

[01:46] Mas nessa pasta que eu acabei de criar, qualquer usuário tem permissão, então vamos nessa. Eu vou acessar os meus serviços - vim /usr/local/etc/nginx/servers/microsservicos.conf.


- No /var/log não rola, porque não tem permissão, quem manda é o root:

~~~~bash
root@debian10x64:/etc/nginx/sites-enabled# ls -lhasp /var/ | grep log
4.0K drwxr-xr-x 16 root root  4.0K Nov 11 00:00 log/
root@debian10x64:/etc/nginx/sites-enabled#
~~~~





https://docs.nginx.com/nginx/admin-guide/monitoring/logging/
https://www.digitalocean.com/community/tutorials/nginx-access-logs-error-logs



- O diretório /var/log/nginx/ pode ser utilizado, pois o NGINX consegue gravar nele

~~~~bash

root@debian10x64:/etc/nginx/sites-enabled# ls -lhasp /var/log/nginx/
total 124K
4.0K drwxr-xr-x  2 root     adm  4.0K Nov  8 23:56 ./
4.0K drwxr-xr-x 16 root     root 4.0K Nov 11 00:00 ../
8.0K -rw-r-----  1 www-data adm  5.5K Nov 11 17:01 access.log
4.0K -rw-r-----  1 www-data adm  1.4K Nov  5 00:05 access.log.1
4.0K -rw-r-----  1 www-data adm   231 Sep 11 00:06 access.log.10.gz
4.0K -rw-r-----  1 www-data adm   272 Sep  9 23:30 access.log.11.gz
4.0K -rw-r-----  1 www-data adm   303 Nov  4 22:44 access.log.2.gz
4.0K -rw-r-----  1 www-data adm   202 Oct 29 00:13 access.log.3.gz
4.0K -rw-r-----  1 www-data adm   372 Oct 22 00:31 access.log.4.gz
4.0K -rw-r-----  1 www-data adm   312 Oct 21 23:15 access.log.5.gz
4.0K -rw-r-----  1 www-data adm   165 Sep 24 15:29 access.log.6.gz
4.0K -rw-r-----  1 www-data adm   190 Sep 23 15:21 access.log.7.gz
4.0K -rw-r-----  1 www-data adm   198 Sep 22 21:14 access.log.8.gz
4.0K -rw-r-----  1 www-data adm   302 Sep 15 21:18 access.log.9.gz
8.0K -rw-r-----  1 www-data adm  4.8K Nov 11 17:01 error.log
4.0K -rw-r-----  1 www-data adm   810 Nov  5 00:05 error.log.1
4.0K -rw-r-----  1 www-data adm   122 Oct 10 21:00 error.log.10.gz
4.0K -rw-r-----  1 www-data adm   122 Oct  8 20:40 error.log.11.gz
4.0K -rw-r-----  1 www-data adm   125 Oct  7 19:27 error.log.12.gz
4.0K -rw-r-----  1 www-data adm   122 Sep 26 21:03 error.log.13.gz
4.0K -rw-r-----  1 www-data adm   260 Sep 24 15:29 error.log.14.gz
4.0K -rw-r-----  1 www-data adm   403 Nov  4 22:44 error.log.2.gz
4.0K -rw-r-----  1 www-data adm   246 Oct 29 00:13 error.log.3.gz
4.0K -rw-r-----  1 www-data adm   545 Oct 22 00:31 error.log.4.gz
4.0K -rw-r-----  1 www-data adm   390 Oct 21 23:15 error.log.5.gz
4.0K -rw-r-----  1 www-data adm   122 Oct 17 21:09 error.log.6.gz
4.0K -rw-r-----  1 www-data adm   123 Oct 14 19:18 error.log.7.gz
4.0K -rw-r-----  1 www-data adm   142 Oct 12 13:00 error.log.8.gz
4.0K -rw-r-----  1 www-data adm   121 Oct 11 11:10 error.log.9.gz
root@debian10x64:/etc/nginx/sites-enabled# date
Sat 11 Nov 2023 05:30:01 PM -03
root@debian10x64:/etc/nginx/sites-enabled#

~~~~





- Exemplo de log_format:

<https://docs.nginx.com/nginx/admin-guide/monitoring/logging/>

~~~~conf
http {
    log_format compression '$remote_addr - $remote_user [$time_local] '
                           '"$request" $status $body_bytes_sent '
                           '"$http_referer" "$http_user_agent" "$gzip_ratio"';

    server {
        gzip on;
        access_log /spool/logs/nginx-access.log compression;
        ...
    }
}

~~~~






- Sobre o "http_x_forwarded_for":

<https://djangocas.dev/blog/nginx/nginx-access-log-with-real-x-forwarded-for-ip-instead-of-proxy-ip/>

Solution 1: Get client user real IP in nginx access_log

In today’s web, a lot web server use CDN, it is useful to log client user’s real IP instead of CDN server IP. Fortunately, CDN servers send request with X-Forwarded-For header including client user’s real IP. We can use X-Forwarded-For header’s value in log.

To do this, first need define a log format in main nginx config. For example, in debian the nginx main config file is /etc/nginx/nginx.conf. Add a log_format config inside http block:

~~~~conf
http {
    ...

    log_format combined_realip '$http_x_forwarded_for - $remote_user [$time_local] '
                    '"$request" $status $body_bytes_sent '
                    '"$http_referer" "$http_user_agent"';

    # For cloudflare argo tunnel, use $http_cf_connecting_ip to get `cf-connecting-ip` header
    log_format combined_realip_cf '$http_cf_connecting_ip $http_x_forwarded_for - $remote_user [$time_local] '
                    '"$request" $status $body_bytes_sent '
                    '"$http_referer" "$http_user_agent"';
}
~~~~








git status
git add .
git commit -m "Módulo 5 - Load Balancer. Aula 01 Upstreams. Ajustes no nginx.conf, adicionando log_format."
eval $(ssh-agent -s)
ssh-add /home/fernando/.ssh/chave-debian10-github
git push
git status





- Editado:

serão desativados os access_log "gerais", porque teremos access_log separados para cada serviço

~~~~conf
        #access_log /var/log/nginx/access.log;
        error_log /var/log/nginx/error.log;

        log_format compression '$remote_addr - $remote_user [$time_local] '
                                '"$request" $status $body_bytes_sent "$http_referer" '
                                '"$http_user_agent" "$http_x_forwarded_for"';
~~~~









- Adicionando o caminho dos logs personalizados para cada serviço:
/home/fernando/cursos/nginx-alura/005-Load-Balancer/sites/microservicos.conf

access_log /var/log/nginx/servico1.log;

access_log /var/log/nginx/servico2.log;


~~~~conf
server {
        listen 8001;
        server_name localhost;

        access_log /var/log/nginx/servico1.log;

        location / {
            root /home/fernando/cursos/nginx-alura/005-Load-Balancer/arquivos/servico1;
            index index.html;
        }

}

server {
        listen 8002;
        server_name localhost;

        access_log /var/log/nginx/servico2.log;

        location / {
            root /home/fernando/cursos/nginx-alura/005-Load-Balancer/arquivos/servico2;
            index index.html;
        }

}
~~~~







- Ajustando nginx.conf e microservicos.conf

~~~~bash

root@debian10x64:/etc/nginx/sites-enabled# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled# nginx -s reload
root@debian10x64:/etc/nginx/sites-enabled#

~~~~





# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
## RESUMO

- 
- O diretório /var/log/nginx/ pode ser utilizado, pois o NGINX consegue gravar nele
- Necessário ativar o log_format no arquivo nginx.conf