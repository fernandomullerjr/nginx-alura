
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# Git

git status
git add .
git commit -m "Módulo 4 - API Gateway. Aula 03 Usando proxy_pass."
eval $(ssh-agent -s)
ssh-add /home/fernando/.ssh/chave-debian10-github
git push
git status



# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# 03 Usando proxy_pass

## Transcrição

[00:00] Vamos criar dois serviços de exemplo. Eu vou criar dois novos servidores lá no nosso NGINX e vou criar esses dois novos servidores em um arquivo só, para nós agilizarmos o processo.

[00:15] O que eu vou fazer? Eu vou vir aqui no terminal, pegar o nosso arquivo default, e vou copiar ele para cp /usr/local/etc/nginx/servers/default.conf /usr/local/etc/nginx/servers/microsservicos.conf.

[00:33] Então nesse arquivo ‘microsservicos.conf’ eu vou ter esses nossos dois servidores. Então, vamos lá! Digitei vim /usr/local/etc/nginx/servers/microsservicos.conf.

[00:43] Então aqui eu vou ter dois serviços: um eu vou colocar na porta 8001. Deixe-me numerar as linhas para ficar um pouco mais fácil, com o comando set number. Então eu vou copiar todas as linhas, só que eu não vou precisar da parte do PHP, então deixe-me remover isso daqui. Eu vou copiar essas linhas e colar aqui embaixo.

[01:08] Esse outro serviço vai estar ouvindo a porta 8002. Então tenho dois serviços. Eu vou deixar um na pasta root /Users/vinnicius.dias/Dev/nginx/servico1; e outro na pasta root /Users/vinnicius.dias/Dev/nginx/servico2;.

[01:26] Teoricamente está tudo certo. Vou remover essa error page da última linha, porque eu não quero criar isso. Eu só vou criar a index do ‘servico1’ e a index do ‘servico2’, só isso. Então vou salvar.

[01:37] Vou criar dentro de mkdir Dev/nginx/servico1 Dev/nginx/servico2. Agora eu vou adicionar echo “Serviço 1” > Dev/nginx/servico1/index.html. Então o que eu estou fazendo com essa linha de comando?

[02:02] Eu estou simplesmente adicionando o conteúdo “Serviço 1” dentro desse novo arquivo que vai ser criado. Então criei lá e vou botar o serviço dois também, .echo “Serviço 2” > Dev/nginx/servico2/index.html.

[02:13] Então deixe-me garantir que não escrevi nada errado... Aparentemente está tudo certo. Agora vou usar o comando nginx -s reload. Teoricamente eu tenho dois serviços, vamos testá-los.

[02:22] No navegador, acesse http://localhost:8001. Está com problema de caractere por causa de acento e eu não ter definido o charset, problema nenhum. E ao acessar http://localhost:8002 temos o serviço 2. Então eu tenho dois serviços.

[02:35] Agora o que eu quero fazer é fazer que a partir do nosso localhost: 8080 (que é aquele nosso servidor padrão), eu redirecione. Quando eu chamar “serviço 1”, manda para o nosso localhost: 8001; quando chamar “serviço 2” manda para o localhost: 8002. Deixe-me ver aqui, isso aqui já está funcionando porque dentro da pasta “nginx”, eu tenho lá a pasta “servico2”, então ele buscou o arquivo “index.html”.

[03:10] Eu vou remover isso daqui. Deixe-meu abrir o nosso o usr com o comando vim /usr/local/etc/nginx/nginx.conf. Eu não vou ter mais isso daqui #proxy_pass http://localhost;. Eu não vou passar mais aquele nosso proxy para o localhost:80, não vou ter mais aquele proxy.

[03:31] Então quando eu fizer o reload agora, com o comando nginx -s reload, a página http://localhost:8002/servico2 não vai mais funcionar.

[03:39] Agora o que eu quero fazer é mandar o que está em “/servico2” para o localhost: 8002, e o que está em “servico1” para o localhost: 8001. Então vamos fazer essa configuração, que não vai ser nada difícil.

[03:51] Vou voltar para o terminal e usar o comando ‘vim /usr/local/etc/nginx/nginx.conf’. Eu posso até copiar a linha de location se eu quiser. Vou colar e vou colar de novo aqui.

[04:00] Quando eu acessar location /servico1 { o que eu quero fazer é um proxy_pass http://localhost:8001/;. Aqui eu vou adicionar a barra no final, por quê? Você já vai entender.

[04:15] Deixe-me voltar para o último comando location - que será o location /servico2 { - proxy_pass http://localhost:8002/;.

[04:23] E agora deixe-me te explicar o motivo para aqui na segunda linha do primeiro location eu não ter a barra no final e nessa última linha de location eu ter.

[04:31] Quando eu acessar ‘/servico1’, eu quero mandar para localhost: 8001. Só que se eu digitar ‘/servico1/teste’, eu quero mandar para “localhost:8001/teste” e não para “localhost:8001/servico/teste”.

[04:48] Então se eu remover essa barra aqui, ele vai adicionar o que passamos no location aqui no final. Já, se eu coloco a barra, ele vai ignorar o que recebeu no location do servidor original e mandar só a partir de “servico1”.

[05:02] Então, assim nós teoricamente já temos configurado!

[05:11] Vamos fazer o comando nginx -t para garantirmos que está tudo certo. Em seguida, o comando nginx -s reload. Agora, teoricamente, quando eu acessar no navegador http://localhost:8080/servico1, ele vai acessar o “Serviço 1”; e quando eu vier em http://localhost:8080/servico2, ele vai acessar o “Serviço 2”.

[05:32] Deixe-me renomear com o comando mv Dev/nginx/servico2/index.htlm Dev/nginx/servico2/servico.html. Ao acessar no navegador http://localhost:8080/servico2, ele não tem mais a pasta “index”. Então eu vou acessar http://localhost:8080/servico2/servico.html e tudo continuará funcionando.

[05:51] Porque sempre que eu acesso o “/servico2”, o que vem depois vai ser mandado para aquele nosso servidor localhost: 8002. Então seria a mesma coisa que eu fazer isso daqui.

[06:03] O que fizemos agora foi adicionarmos um servidor que nos redireciona para serviços diferentes e que realizam tarefas específicas.

[06:12] Então, eu poderia, por exemplo, aqui na nossa configuração (imagine aquele seu servidor ‘api.alura.com.br’) colocar aqui na localização ‘/financeiro’ e eu mandar para o nosso serviço de financeiro; ‘/academico’. Eu mandaria para o servidor que tem o serviço acadêmico.

[06:32] Então, dessa forma, eu teria um ponto de entrada para acessar vários servidores. Isso tem um nome bastante específico – e é sobre esse nome e essa técnica que eu vou falar no próximo vídeo!







# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# 03 Usando proxy_pass


- Criar arquivo de conf que vai ter os 2 microserviços:

/home/fernando/cursos/nginx-alura/004-API-Gateway/sites/microservicos.conf

~~~~conf
server {
        listen 8001;
        server_name localhost;

        location / {
            root /home/fernando/cursos/nginx-alura/004-API-Gateway/arquivos/servico1;
            index index.html;
        }

}

server {
        listen 8002;
        server_name localhost;

        location / {
            root /home/fernando/cursos/nginx-alura/004-API-Gateway/arquivos/servico2;
            index index.html;
        }

}
~~~~



- Criados os arquivos html
/home/fernando/cursos/nginx-alura/004-API-Gateway/arquivos/servico1/index.html
/home/fernando/cursos/nginx-alura/004-API-Gateway/arquivos/servico2/index.html

- Ajustando no sites-enabled do NGINX:

~~~~BASH

root@debian10x64:/etc/nginx/sites-enabled# vi microservicos.conf
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled# nginx -s reload
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled# date
Sat 04 Nov 2023 10:18:23 PM -03
root@debian10x64:/etc/nginx/sites-enabled# ls
microservicos.conf  site-aula-nginx.conf  teste-proxy-reverso.conf
root@debian10x64:/etc/nginx/sites-enabled#

~~~~





- Testando

~~~~bash

root@debian10x64:/etc/nginx/sites-enabled# curl http://192.168.0.110:8001/
Serviço 1
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled# curl http://192.168.0.110:8002
Serviço 2
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled#

~~~~










[02:35] Agora o que eu quero fazer é fazer que a partir do nosso localhost: 8080 (que é aquele nosso servidor padrão), eu redirecione. Quando eu chamar “serviço 1”, manda para o nosso localhost: 8001; quando chamar “serviço 2” manda para o localhost: 8002. Deixe-me ver aqui, isso aqui já está funcionando porque dentro da pasta “nginx”, eu tenho lá a pasta “servico2”, então ele buscou o arquivo “index.html”.



- Necessário ajustar o nginx.conf, comentando o proxy_pass nele.
- Verificando o nginx.conf, ele não tem a linha contendo "proxy_pass".
- O "proxy_pass" para o localhost existe nestes 2 arquivos apenas:
site-aula-nginx.conf
teste-proxy-reverso.conf


- Acessando a url mencionada no video, ele joga para página de erro, ao invés de abrir a página do "Serviço 2", conforme ocorria com o professor no ambiente dele:
http://192.168.0.110:8080/servico2/index.html
<http://192.168.0.110:8080/servico2/index.html>


root@debian10x64:/etc/nginx/sites-enabled# curl http://192.168.0.110:8080/servico2/index.html
<!DOCTYPE html>
<html>
<head>
<title>Erro!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Erro ao tentar acessar este caminho.</h1>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>root@debian10x64:/etc/nginx/sites-enabled#


