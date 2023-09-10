
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# Git

git status
git add .
git commit -m "Módulo 2 - Conhecendo a ferramenta. Aula  01 Arquivos de configuração."
eval $(ssh-agent -s)
ssh-add /home/fernando/.ssh/chave-debian10-github
git push
git status





# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# 01 Arquivos de configuração
 
## Transcrição

[00:00] E aí, pessoal? Boas-vindas de volta a mais um capítulo desse treinamento, onde vamos começar a brincar um pouco com o NGINX. Vamos entender como funciona um servidor web.

[00:09] E como eu comentei, isso aqui já é um servidor web rodando, ele já está nos respondendo de forma válida. Então vamos entender o que ele fez e o que ele já tem por padrão.

[00:20] Então vou vir aqui no meu terminal e eu vou digitar o comando nginx -h. O que isso vai nos mostrar? Vai nos mostrar a ajuda. Ele mostra primeiro a versão que eu estou utilizando, o uso que eu posso fazer aqui e todas as opções que eu posso passar.

[00:37] Eu tenho algumas informações interessantes aqui. Primeiro: ele mostra no meu sistema operacional qual é o caminho de prefixo, o caminho onde tem as configurações e onde tem o que eu preciso conhecer.

[00:50] Então repare aqui, dentro desse arquivo eu devo ter provavelmente aqueles nossos arquivos que estam gerando essa página aqui. Posso ter mais arquivo de configuração etc.

[01:04] Eu tenho aqui arquivos de log, para onde as coisas vão ser salvas. Eu tenho arquivos de configuração padrão, então repare que aqui eu já vejo um arquivo de configuração padrão. Então acho que esse é um lugar interessante para nós começarmos.

[01:19] No seu computador esse caminho pode ser diferente. Então, através dessa informação é que vamos entender o que vamos analisar. Você vai abrir esse arquivo padrão em qualquer editor de texto. Eu aqui vou abrir no ‘vim’ para não precisar sair do terminal.

[01:35] Então vou digitar o comando vim /usr/loca/etc/nginx/nginx.conf. Então vamos entender o que está acontecendo. Aqui você vai reparar que tem um monte de linhas que começam com cerquilha (#). Isso significa que essas linhas são ignoradas, elas não têm valor, são somente comentários.

[01:57] Então para nós visualizarmos isso um pouco melhor aqui no meu computador, eu vou fazer um outro comando para ler esse arquivo sem esses comentários. Mas você pode manter seu editor de texto aberto que vamos editar por lá.

[02:09] Então já tem os comando aqui e essas são as configurações realmente carregadas, sem espaço em branco, sem os comentários etc.

[02:17] Então repare que temos bastante coisas e podemos até ficar perdidos em algumas coisas. Eu tenho aqui o número de ‘worker_processes’, então aqui por padrão ele está colocando só ‘1’ para mim.

[02:31] Eu posso mudar essa configuração para ‘auto’, e provavelmente na sua instalação de Linux isso já veio como ‘auto’, mas eu vou manter esse ‘1’ porque nós não vamos tratar de performance nesse treinamento.

[02:43] Aqui ele nos diz quantas requisições e conexões o mesmo ‘worker’ pode receber. Isso aqui é o limite que um processo pode ter de file descriptors. É o limite que um processo pode ter, no meu sistema operacional, de coisas assíncronas, vamos chamar assim.

[03:01] Então no Linux e em sistemas Unix no geral, tudo é considerado um arquivo. Então se eu tenho socket, se eu tenho uma pasta ou um processo, tudo pode ser visto como um arquivo. Então isso aqui é o número de file descriptors, ou seja, descritores de arquivo; para cada processo poder trabalhar.

[03:19] Então podem ser socket, podem ser conexões usando outra forma ou alguma outra técnica, mas no final das contas esse é o número de conexões que eu posso ter. Eu posso mexer no meu sistema operacional para aumentar isso - mas de novo, não vou mexer por agora.

[03:33] E agora entramos na parte de configuração do HTTP propriamente dito. Então, o que nós temos aqui? Ele faz o ‘include’ de outro arquivo, não vou me preocupar com isso. Ele coloca aqui que se nenhum tipo for definido na resposta, ele vai colocar esse tipo aqui. Não vamos nos preocupar com isso. Passa outras informações, mas esse é o ponto principal, é onde ele configura o nosso servidor web.

[03:57] Então repare que ele está ouvindo a porta 8080, em um nome de servidor ‘localhost’.

[04:03] Se tivéssemos algum host configurado na nossa máquina, ou seja, no arquivo de hosts do sistema operacional. Se disséssemos ‘alura.com.br’ iria ser direcionado para nossa máquina. Então aqui se colocássemos ‘alura.com.br’, ele conseguiria pegar essa requisição e tratar diretamente aqui no nosso servidor.

[04:24] Não vamos mudar isso, vamos utilizar ‘localhost’, mas em um servidor real aqui no server_name você coloca o nome do domínio do seu site.

[04:34] Aqui em 'location' vem a configuração que indica o que fazer quando ele receber alguma requisição. Se essa requisição for para ‘/’, ou seja, para alguma coisa que não tem caminho nenhum, então ele vai buscar dentro da pasta HTML o que nós informarmos.

[04:51] E se nós não informarmos nada, se realmente a requisição for só ‘/’ e não ‘/ mais alguma coisa’, caso não informemos nada o que ele vai fazer é buscar um arquivo de índice, um arquivo principal que pode se chamar ‘index.html’ ou 'index. htm'.

[05:08] Então, com isso, o que eu tenho de informação? Eu sei que dentro da pasta HTML que está naquela pasta de prefixo que nós vimos, eu preciso ter um arquivo ‘index.html’ que vai ter esse conteúdo aqui da página de boas-vindas.

[05:22] Então, vamos testar isso? Vamos ver de novo qual é aquele caminho - que é isso daqui: '/usr/local/Cellar/nginx/1.19.8/'.

[05:28] Então aqui dentro eu espero ter uma pasta HTML e eu espero que aqui dentro eu tenha um arquivo ‘index’. Então vamos abrir esse arquivo ‘index’ e eu vou abrir com o nosso ‘vim’.

[05:40] Aqui, ao invés de botar ‘Welcome to nginx’, eu vou adicionar aqui nesse título <h1>Welcome to nginx (Teste)!</h1>.

[05:48] Então eu alterei esse conteúdo, salvei e vou atualizar o navegador.

[05:54] Então eu já entendi o que está acontecendo por trás dessa nossa configuração.

[05:58] Agora, um último detalhe é que via de regra, o que vamos fazer é não editar esse arquivo - e eu acredito que no Linux você nem vai ter esse server aqui no arquivo principal.

[06:13] Esse arquivo só vamos editar em cenários de tuning, de otimização e esse tipo de coisa. Mas para configurarmos realmente um servidor, nós vamos acessar essa pasta aqui de ‘servers’.

[06:24] Então aqui dentro nós vamos criar arquivos de configuração específicos para cada servidor que quisermos. Então o que eu vou fazer é criar um novo servidor HTTP, que ao invés de ouvir a porta 8080, vai ouvir a porta padrão HTTP: a porta 80.

[06:40] Se no seu computador esse servidor padrão que o NGINX traz estiver ouvindo a porta 80 então você vai fazer o contrário. Nós vamos criar um novo servidor que ouça a porta 8080.

[06:51] Então nós vamos criar um novo servidor que ouça uma porta diferente e que vá carregar algum outro arquivo que iremos decidir ainda. Só que isso tudo iremos fazer a partir do próximo vídeo.





# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# 01 Arquivos de configuração


nginx -h


~~~~bash

root@debian10x64:/home/fernando# nginx -h
nginx version: nginx/1.14.2
Usage: nginx [-?hvVtTq] [-s signal] [-c filename] [-p prefix] [-g directives]

Options:
  -?,-h         : this help
  -v            : show version and exit
  -V            : show version and configure options then exit
  -t            : test configuration and exit
  -T            : test configuration, dump it and exit
  -q            : suppress non-error messages during configuration testing
  -s signal     : send signal to a master process: stop, quit, reopen, reload
  -p prefix     : set prefix path (default: /usr/share/nginx/)
  -c filename   : set configuration file (default: /etc/nginx/nginx.conf)
  -g directives : set global directives out of configuration file

~~~~


- Nesta saída do help do NGINX ele traz algumas informações, como por exemplo, o caminho onde estão os arquivos por padrão:
      -p prefix     : set prefix path (default: /usr/share/nginx/



- Localização do arquivo de configuração:

/etc/nginx/nginx.conf

~~~~bash
root@debian10x64:/usr/share/nginx# cat /etc/nginx/nginx.conf
user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
        worker_connections 768;
        # multi_accept on;
}

http {

        ##
        # Basic Settings
        ##

        sendfile on;
        tcp_nopush on;
        tcp_nodelay on;
        keepalive_timeout 65;
        types_hash_max_size 2048;
        # server_tokens off;

        # server_names_hash_bucket_size 64;
        # server_name_in_redirect off;

        include /etc/nginx/mime.types;
        default_type application/octet-stream;

        ##
        # SSL Settings
        ##

        ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # Dropping SSLv3, ref: POODLE
        ssl_prefer_server_ciphers on;

        ##
        # Logging Settings
        ##

        access_log /var/log/nginx/access.log;
        error_log /var/log/nginx/error.log;

        ##
        # Gzip Settings
        ##

        gzip on;

        # gzip_vary on;
        # gzip_proxied any;
        # gzip_comp_level 6;
        # gzip_buffers 16 8k;
        # gzip_http_version 1.1;
        # gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

        ##
        # Virtual Host Configs
        ##

        include /etc/nginx/conf.d/*.conf;
        include /etc/nginx/sites-enabled/*;
}


#mail {
#       # See sample authentication script at:
#       # http://wiki.nginx.org/ImapAuthenticateWithApachePhpScript
#
#       # auth_http localhost/auth.php;
#       # pop3_capabilities "TOP" "USER";
#       # imap_capabilities "IMAP4rev1" "UIDPLUS";
#
#       server {
#               listen     localhost:110;
#               protocol   pop3;
#               proxy      on;
#       }
#
#       server {
#               listen     localhost:143;
#               protocol   imap;
#               proxy      on;
#       }
#}
root@debian10x64:/usr/share/nginx#

~~~~





- Filtrando o nginx conf, trazendo apenas as linhas que não estão comentadas:

cat /etc/nginx/nginx.conf | grep -vE '^\s*#'

~~~~bash
root@debian10x64:/usr/share/nginx# cat /etc/nginx/nginx.conf | grep -vE '^\s*#'
user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
        worker_connections 768;
}

http {


        sendfile on;
        tcp_nopush on;
        tcp_nodelay on;
        keepalive_timeout 65;
        types_hash_max_size 2048;


        include /etc/nginx/mime.types;
        default_type application/octet-stream;


        ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # Dropping SSLv3, ref: POODLE
        ssl_prefer_server_ciphers on;


        access_log /var/log/nginx/access.log;
        error_log /var/log/nginx/error.log;


        gzip on;



        include /etc/nginx/conf.d/*.conf;
        include /etc/nginx/sites-enabled/*;
}


root@debian10x64:/usr/share/nginx#

~~~~








worker_processes auto;

events {
        worker_connections 768;
}


[02:31] Eu posso mudar essa configuração para ‘auto’, e provavelmente na sua instalação de Linux isso já veio como ‘auto’, mas eu vou manter esse ‘1’ porque nós não vamos tratar de performance nesse treinamento.

[02:43] Aqui ele nos diz quantas requisições e conexões o mesmo ‘worker’ pode receber. Isso aqui é o limite que um processo pode ter de file descriptors. É o limite que um processo pode ter, no meu sistema operacional, de coisas assíncronas, vamos chamar assim.











- Verificando a configuração default do sites-enabled

~~~~bash

root@debian10x64:/etc/nginx# cat sites-enabled/default
##
# You should look at the following URL's in order to grasp a solid understanding
# of Nginx configuration files in order to fully unleash the power of Nginx.
# https://www.nginx.com/resources/wiki/start/
# https://www.nginx.com/resources/wiki/start/topics/tutorials/config_pitfalls/
# https://wiki.debian.org/Nginx/DirectoryStructure
#
# In most cases, administrators will remove this file from sites-enabled/ and
# leave it as reference inside of sites-available where it will continue to be
# updated by the nginx packaging team.
#
# This file will automatically load configuration files provided by other
# applications, such as Drupal or Wordpress. These applications will be made
# available underneath a path with that package name, such as /drupal8.
#
# Please see /usr/share/doc/nginx-doc/examples/ for more detailed examples.
##

# Default server configuration
#
server {
        listen 80 default_server;
        listen [::]:80 default_server;

        # SSL configuration
        #
        # listen 443 ssl default_server;
        # listen [::]:443 ssl default_server;
        #
        # Note: You should disable gzip for SSL traffic.
        # See: https://bugs.debian.org/773332
        #
        # Read up on ssl_ciphers to ensure a secure configuration.
        # See: https://bugs.debian.org/765782
        #
        # Self signed certs generated by the ssl-cert package
        # Don't use them in a production server!
        #
        # include snippets/snakeoil.conf;

        root /var/www/html;

        # Add index.php to the list if you are using PHP
        index index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;
        }

        # pass PHP scripts to FastCGI server
        #
        #location ~ \.php$ {
        #       include snippets/fastcgi-php.conf;
        #
        #       # With php-fpm (or other unix sockets):
        #       fastcgi_pass unix:/run/php/php7.3-fpm.sock;
        #       # With php-cgi (or other tcp sockets):
        #       fastcgi_pass 127.0.0.1:9000;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #       deny all;
        #}
}


# Virtual Host configuration for example.com
#
# You can move that to a different file under sites-available/ and symlink that
# to sites-enabled/ to enable it.
#
#server {
#       listen 80;
#       listen [::]:80;
#
#       server_name example.com;
#
#       root /var/www/example.com;
#       index index.html;
#
#       location / {
#               try_files $uri $uri/ =404;
#       }
#}
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx#

~~~~








- Trazendo o default sem os comentários:

~~~~bash

root@debian10x64:/etc/nginx# cat sites-enabled/default | grep -vE '^\s*#'

server {
        listen 80 default_server;
        listen [::]:80 default_server;


        root /var/www/html;

        index index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
                try_files $uri $uri/ =404;
        }


}


root@debian10x64:/etc/nginx#

~~~~


[04:34] Aqui em 'location' vem a configuração que indica o que fazer quando ele receber alguma requisição. Se essa requisição for para ‘/’, ou seja, para alguma coisa que não tem caminho nenhum, então ele vai buscar dentro da pasta HTML o que nós informarmos.

[04:51] E se nós não informarmos nada, se realmente a requisição for só ‘/’ e não ‘/ mais alguma coisa’, caso não informemos nada o que ele vai fazer é buscar um arquivo de índice, um arquivo principal que pode se chamar ‘index.html’ ou 'index. htm'.

[05:08] Então, com isso, o que eu tenho de informação? Eu sei que dentro da pasta HTML que está naquela pasta de prefixo que nós vimos, eu preciso ter um arquivo ‘index.html’ que vai ter esse conteúdo aqui da página de boas-vindas.

[05:22] Então, vamos testar isso? Vamos ver de novo qual é aquele caminho - que é isso daqui: '/usr/local/Cellar/nginx/1.19.8/'.

[05:28] Então aqui dentro eu espero ter uma pasta HTML e eu espero que aqui dentro eu tenha um arquivo ‘index’. Então vamos abrir esse arquivo ‘index’ e eu vou abrir com o nosso ‘vim’.






- Verificando o HTML que é carregado pelo default

~~~~bash
root@debian10x64:/var/www/html# ls
index.nginx-debian.html
root@debian10x64:/var/www/html# pwd
/var/www/html
root@debian10x64:/var/www/html#

root@debian10x64:/var/www/html# cat index.nginx-debian.html
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
root@debian10x64:/var/www/html#

~~~~






- Conteúdo do arquivo HTML:

~~~~html
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
~~~~












