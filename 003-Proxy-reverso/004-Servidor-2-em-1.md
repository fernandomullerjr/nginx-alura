
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# Git

git status
git add .
git commit -m "Módulo 3 - Proxy Reverso. Aula 04 Servidor 2 em 1."
eval $(ssh-agent -s)
ssh-add /home/fernando/.ssh/chave-debian10-github
git push
git status



# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
#  04 Servidor 2 em 1

## Transcrição

[00:00] Agora vamos ver uma utilidade, algo um pouco mais real para nós utilizarmos o proxy reverso do NGINX.

[00:08] Imagine o seguinte cenário: eu tenho um servidor de aplicação rodando alguma coisa e eu tenho também, além de uma aplicação, arquivos no meu sistema. Então, por exemplo: o site da Alura.

[00:21] Eu tenho algum servidor rodando em alguma linguagem, que vai fazer a lógica para verificar quais cursos estão cadastrados no banco de dados, as novidades que foram inseridas, promoções etc.

[00:32] Mas além dessa lógica, eu também tenho arquivos estáticos. Arquivos de CSS para estilizar a página, imagens, o próprio HTML... Enfim. Dessa forma, o que eu tenho?

[00:44] Eu tenho arquivos estáticos que não precisam de processamento nenhum, eu só recebo essa requisição e posso devolver esses arquivos muito rápido; e tenho algumas coisas, algumas URLs, alguns locations que precisam ser processados por um servidor de aplicação.

[01:00] Então, podemos usar o proxy reverso para recebermos requisição e devolvermos muito rápido se for algum arquivo estático - inclusive fazer cache e esse tipo de coisas. Agora, se for alguma rota, alguma URL que precise de lógica e que precisa ser processada, eu mando para o nosso servidor de aplicação.

[01:19] Então o que eu quero fazer é, por exemplo: eu tenho aqui o meu localhost, tenho configurado esse servidor. Eu quero que quando eu tentar acessar alguma coisa .php, ele mande para um servidor que está rodando PHP.

[01:33] Agora se for qualquer coisa que não termine com .php, nós mantemos a nossa regra que já tem lá. Então vamos ver como que podemos fazer isso.

[01:40] Eu vou voltar para o terminal. Vou configurar aquele nosso site que está dentro de ‘servers’, que no meu computador é aquele que temos na porta padrão ‘:80’. Então vou configurar ele aqui que está ouvindo a porta ‘80’ e eu vou adicionar um novo ‘location’. Aqui vamos aprender uma nova regra interessante.

[02:02] Aqui em location, como eu comentei, além de ter strings chapadas eu posso ter expressões regulares. Então vou ter aqui, através do acento til (~), eu indico que eu vou ter uma expressão regular e que ele leve em consideração letras maiúsculas e minúsculas. Se eu quiser ignorar letra maiúscula e minúscula, eu posso usar til e asterisco (~*).

[02:24] Então, o que eu quero fazer é pegar a location de qualquer coisa que termine com .php, location ~ \.php. Então eu preciso colocar essa barra invertida aqui porque o ponto (.), em uma expressão regular, significa qualquer carácter. Então eu boto essa barra invertida () para indicar que não é qualquer caracter, é o caractere ponto mesmo.

[02:42] Eu posso indicar ainda que isso deve ser o final desse ‘location’, adicionando o cifrão ($). Dessa forma eu tenho um novo ‘location’.

[02:48] Aqui o que eu quero fazer é um proxy reverso. Eu quero que quando alguma requisição chegar para algo que termine com ‘.php’, eu vou mandar para o servidor de PHP e ele vai processar isso aqui.

[03:00] Então eu posso usar o comando proxy_pass http://localhost:8000. Dessa forma, eu tenho dois ‘locations’. Se é alguma imagem, se é algum arquivo estático, o NGINX já vai devolver diretamente, ou seja, isso vai ser muito rápido. Agora se for alguma algum arquivo PHP, o que ele vai fazer é acessar o servidor de PHP que vai fazer o processamento que tem que fazer.

[03:30] Se você não conhece PHP, você pode usar qualquer outra regra aqui. Você pode usar, por exemplo, um Ngrok para servir; você pode usar o HTTP Server do Node; você pode subir um Tomcat para o Java - mas aqui, para manter muito simples eu vou utilizar o PHP.

[03:46] Não precisa se preocupar em reproduzir exatamente isso. Se você tiver o PHP na sua máquina, faça junto comigo; senão você pode adaptar isso para a sua linguagem.

[03:54] Mas continuando, que eu já estou falando bastante! Vou salvar isso aqui e fazer o reload da configuração. Só que você deve estar se perguntando: “Mas e o servidor que está lá na porta 8000 de PHP?”

[04:06] Então vou subir ele aqui mesmo, sem ter arquivo nenhum, eu vou subir em php -S localhost:8000. Então, teoricamente, sempre que eu fizer uma requisição “/algumacoisa.php’ ele vai mandar para esse servidor aqui de PHP. Como eu não tenho nenhum arquivo PHP, ele vai mostrar que não foi possível encontrar o arquivo.

[04:25] Então, vamos lá! Eu vou tentar acessar no navegador http://localhost/algum.php. E quando eu acesso, ele bate no servidor HTTP.

[04:34] Repare que a resposta é diferente, e se eu vejo aqui no terminal ele acessou realmente o nosso servidor PHP.

[04:40] Então essa é uma aplicação pouco mais real de um proxy reverso. Se nós temos arquivos estáticos, simplesmente devolve. Não precisamos passar por um servidor de aplicação para isso.

[04:50] Agora, se é algo que demanda processamento, nesse caso sim, nós vamos fazer o proxy reverso e mandar para um servidor de aplicação.

[04:58] Quero deixar bem claro que essa não é a única forma de mandarmos para um servidor de aplicação, eu estou mandando diretamente de uma outra requisição HTTP. Só que existem outros protocolos que nós poderíamos utilizar, mas isso pode ser assunto para o futuro.

[05:12] Aqui já vimos na prática como termos uma utilização real de um proxy reverso, mas essa não é nem a única forma de implementarmos e nem a única utilidade. Então no próximo capítulo vamos falar de um cenário bastante real, onde utilizamos proxy reverso.




# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
#  04 Servidor 2 em 1

- Podemos utilizar o NGINX para encaminhar as requisições recebidas para
Arquivos estáticos
Servidores de aplicação




[02:02] Aqui em location, como eu comentei, além de ter strings chapadas eu posso ter expressões regulares. Então vou ter aqui, através do acento til (~), eu indico que eu vou ter uma expressão regular e que ele leve em consideração letras maiúsculas e minúsculas. Se eu quiser ignorar letra maiúscula e minúscula, eu posso usar til e asterisco (~*).

[02:24] Então, o que eu quero fazer é pegar a location de qualquer coisa que termine com .php, location ~ \.php. Então eu preciso colocar essa barra invertida aqui porque o ponto (.), em uma expressão regular, significa qualquer carácter. Então eu boto essa barra invertida () para indicar que não é qualquer caracter, é o caractere ponto mesmo.

[02:42] Eu posso indicar ainda que isso deve ser o final desse ‘location’, adicionando o cifrão ($). Dessa forma eu tenho um novo ‘location’.





- Criado arquivo NGINX para o site na porta 8080, contendo o novo location.
- Este location efetua o proxy_pass para a aplicação em PHP na porta 8000.
- A expressão regular é lida devido o til ~ ao inicio do location.
- A barra invertida é necessária para que o ponto deve ser considerado como ponto mesmo, ao invés de significar qualquer caracter, devido a expressão regular.

~~~~conf

server {
        listen 8080;
        server_name localhost;

        location / {
            root /home/fernando/cursos/nginx-alura/002-Servidor-HTTP/arquivos-para-aula;
            index index.html;
        }

        location ~ \.php$ {
            proxy_pass http://localhost:8000;
        }

        error_page 404 400 401 /erro.html;
}

~~~~




- Instalando o PHP 8, para poder subir uma aplicação em PHP:

https://computingforgeeks.com/how-to-install-php-on-debian-linux/

sudo apt update
sudo apt install -y lsb-release ca-certificates apt-transport-https software-properties-common gnupg2
echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/sury-php.list
wget -qO - https://packages.sury.org/php/apt.gpg | sudo apt-key add -
sudo apt install php8.0



- Subindo aplicação de teste:

php -S localhost:8000

~~~~bash
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled# php -S localhost:8000
[Sun Oct 22 00:29:51 2023] PHP 8.0.30 Development Server (http://localhost:8000) started
~~~~






- Ajustando o site nginx, com o location que utiliza expressão regular:

~~~~bash

root@debian10x64:/etc/nginx/sites-enabled# vi site-aula-nginx.conf
root@debian10x64:/etc/nginx/sites-enabled# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
root@debian10x64:/etc/nginx/sites-enabled# nginx -s reload
root@debian10x64:/etc/nginx/sites-enabled#
~~~~





- Efetuando teste, acessando uma página php inexistente:

http://192.168.0.110:8080/algum.php

~~~~bash
root@debian10x64:/etc/nginx/sites-enabled# curl http://192.168.0.110:8080/algum.php
<!doctype html><html><head><title>404 Not Found</title><style>
body { background-color: #fcfcfc; color: #333333; margin: 0; padding:0; }
h1 { font-size: 1.5em; font-weight: normal; background-color: #9999cc; min-height:2em; line-height:2em; border-bottom: 1px inset black; margin: 0; }
h1, p { padding-left: 10px; }
code.url { background-color: #eeeeee; font-family:monospace; padding:0 2px;}
</style>
</head><body><h1>Not Found</h1><p>The requested resource <code class="url">/algum.php</code> was not found on this server.</p></body></html>root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled#
~~~~