
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# Git

git status
git add .
git commit -m "Módulo 2 - Servidor HTTP. Aula 06 Páginas de erro."
eval $(ssh-agent -s)
ssh-add /home/fernando/.ssh/chave-debian10-github
git push
git status



# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# 06 Páginas de erro

## Transcrição

[00:00] Infelizmente quando desenvolvemos uma aplicação, um site ou um sistema, erros acontecem. Quando estamos falando de HTTP, erros são representados através de códigos HTTP. De novo, por isso é importante fazermos o treinamento de HTTP, para entendermos melhor como isso funciona, mas eu vou super simplificar um conceito aqui.

[00:22] Se aconteceu algum erro e estamos utilizando bem o HTTP, um código de resposta é devolvido. Então, o código de erro “404” indica que algum recurso que eu estou tentando acessar não foi encontrado.

[00:39] O código “200” indica que está tudo OK, que nós encontramos o que tinha que encontrar e processamos o que tinha que processar. Então existem diversos códigos HTTP.

[00:50] Os códigos de erro estão na faixa “400” quando é algum erro do cliente, ou seja, o cliente digitou a URL errada, por exemplo. Ou na faixa “500” quando é erro no servidor. Então, por exemplo: o servidor não está disponível, o servidor de aplicação caiu, meu banco de dados está fora ou alguma coisa assim.

[01:07] Então vamos configurar exatamente o erro “404” para ser direcionado para uma página nossa, uma página que nós podemos configurar como quisermos, deixar ela bonita etc.

[01:17] Então, vamos lá! Já estou aqui com meu arquivo de configuração aberto e fora de location eu vou adicionar o comando error_page - pode ser antes ou depois, tanto faz.

[01:28] E aqui, o que eu posso informar? Os códigos de erro, por exemplo: “404”, “400”, “401” etc. - e isso vai ser direcionado para ‘/erro.html’, error_page 404 400 401 /erro.html.

[01:42] Então, o que vai acontecer aqui? Algum desses erros (404, 400 ou 401) aconteceu, então o NGINX vai carregar como se ele estivesse acessando uma nova requisição HTTP para ‘/erro.html’. Então, o que eu espero? Que dentro desse ‘root’ aqui eu tenha esse arquivo ‘erro.html’.

[02:02] Então vamos salvar esse arquivo de configuração. Primeiro, eu vou garantir que está tudo certo e OK, se a sintaxe está certa. Então eu vou lá no meu ‘root’, que é em ‘vim Dev/nginx/error.html’. Eu vou escrever aqui "mensagem de erro", ou "código de erro", e vou poder fazer o que eu quiser aqui.

[02:20] De novo, se eu tento acessar pelo navegador alguma coisa não encontrada, ele não vai carregar aquele arquivo, porque precisamos recarregar as configurações. Então eu digito o comando nginx -s reload. Configurações recarregadas quando eu atualizo... "Mensagem de erro". Por quê?

[02:36] Nós tentamos acessar ‘/alguma-coisa’. Esse ‘/alguma-coisa’ não existe, então o NGINX vai identificar o erro “404”. O próprio NGINX já sabe entender isso. Então, quando acontece um erro “404” ou algum daqueles outros que nós colocamos, ele vai reagir como se fosse refazer a requisição para ‘/erro.html’ – e quando acessamos ‘/erro.html’ é exatamente essa a resposta que nós temos.

[03:01] Se quisermos testar, podemos acessar no navegador ‘localhost/erro.html’ - que é exatamente isso.

[03:06] Então, dessa forma, conseguimos configurar mensagem de erro. Dessa forma temos, bastante simples, um servidor web rodando. Eu posso facilmente agora criar todo meu site com HTML, CSS, imagens e arquivos em geral.

[03:22] Só que nem só para isso serve o NGINX, não é só para ouvir requisições HTTP e te devolver arquivos. Então vamos começar a entender algumas outras funcionalidades que essa ferramenta tão poderosa nos traz, a partir dos próximos capítulos.






# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# 06 Páginas de erro


- Site conf atual
/home/fernando/cursos/nginx-alura/002-Servidor-HTTP/sites/default_v2.conf

~~~~conf
server {
        listen 8080;
        server_name localhost;

        location / {
            root /home/fernando/cursos/nginx-alura/002-Servidor-HTTP/arquivos-para-aula;
            index index.html;
        }
}
~~~~


- Que ao acessar retorna uma página 404 feia, além de deixar exposta a versão do NGINX:

http://192.168.0.110:8080/alguma-coisa
<http://192.168.0.110:8080/alguma-coisa>
404 Not Found
nginx/1.14.2





[01:17] Então, vamos lá! Já estou aqui com meu arquivo de configuração aberto e fora de location eu vou adicionar o comando error_page - pode ser antes ou depois, tanto faz.

[01:28] E aqui, o que eu posso informar? Os códigos de erro, por exemplo: “404”, “400”, “401” etc. - e isso vai ser direcionado para ‘/erro.html’, error_page 404 400 401 /erro.html.

[01:42] Então, o que vai acontecer aqui? Algum desses erros (404, 400 ou 401) aconteceu, então o NGINX vai carregar como se ele estivesse acessando uma nova requisição HTTP para ‘/erro.html’. Então, o que eu espero? Que dentro desse ‘root’ aqui eu tenha esse arquivo ‘erro.html’.





- Site conf EDITADA, com página de erro personalizada:
/home/fernando/cursos/nginx-alura/002-Servidor-HTTP/sites/default_v3.conf

~~~~conf
server {
        listen 8080;
        server_name localhost;

        location / {
            root /home/fernando/cursos/nginx-alura/002-Servidor-HTTP/arquivos-para-aula;
            index index.html;
        }

        error_page 404 400 401 /erro.html;
}
~~~~



- Ajustando:

~~~~bash

root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx# vi sites-enabled/default-3.conf
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx# cat sites-enabled/default-3.conf
server {
        listen 8080;
        server_name localhost;

        location / {
            root /home/fernando/cursos/nginx-alura/002-Servidor-HTTP/arquivos-para-aula;
            index index.html;
        }

        error_page 404 400 401 /erro.html;
}
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx# nginx -t
nginx: [warn] conflicting server name "localhost" on 0.0.0.0:8080, ignored
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
root@debian10x64:/etc/nginx#
root@debian10x64:/etc/nginx# nginx -s reload
nginx: [warn] conflicting server name "localhost" on 0.0.0.0:8080, ignored
root@debian10x64:/etc/nginx#

~~~~








- Ao tentar acessar este endereço, não traz a página de erro esperada:
http://192.168.0.110:8080/alguma-coisa
404 Not Found
nginx/1.14.2


- Ao tentar acessar o endereço abaixo, traz a página de erro personalizada:
http://192.168.0.110:8080/erro.html

Erro ao tentar acessar este caminho.

For online documentation and support please refer to nginx.org.
Commercial support is available at nginx.com.

Thank you for using nginx.





## PENDENTE
- Ver porque a página de erro só é carregada acessando o html dela direto. Verificar questão dos multiplos sites que tem na sites-enabled.