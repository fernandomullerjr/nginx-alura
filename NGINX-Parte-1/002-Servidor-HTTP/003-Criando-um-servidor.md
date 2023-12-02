
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# Git

git status
git add .
git commit -m "Módulo 2 - Conhecendo a ferramenta. Aula   02 Caminhos dos arquivos."
eval $(ssh-agent -s)
ssh-add /home/fernando/.ssh/chave-debian10-github
git push
git status





# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
#  03 Criando um servidor

Transcrição
[00:00] Vamos finalmente configurar do zero um servidor nosso, não vamos mais utilizar esse servidor aqui que está todo configurado.

[00:09] Então, como vamos fazer isso? Se você estiver no Linux, ao invés de ter esse server aqui, muito provavelmente só tem um include de um outro lugar, de um outro arquivo - e muito provavelmente esse outro lugar tem um arquivo chamado "default.conf".

[00:25] Dependendo da sua instalação, você pode ter pastas conhecidas como sites enabled e sites available, ou somente esse arquivo default.conf. Mas você vai ter todas essas informações dando uma olhada nesse arquivo aqui, 'includ server/', onde você tem os includes.

[00:41] Então dentro dessa pasta que tem o include é que vamos criar um novo arquivo, ou caso você já tenha o default.conf, você pode utilizar ele. Inclusive, eu vou até utilizar esse mesmo nome default.conf para termos algo parecido.

[00:55] Então, eu preciso achar essa pasta servers e esse meu arquivo de configuração está nesse caminho, ‘/usr/local/etc/nginx/’.

[01:01] Então, eu espero que nesse caminho tenha uma pasta servers. Vamos ver aqui. Eu vou utilizar o ‘vim’ para editar algum arquivo e quando eu começo a digitar ‘servers’ e pressiono a tecla “Tab”. Ele completou, então essa pasta existe.

[01:13] Então vou criar um arquivo default.conf. É um arquivo vazio, repare que ele está mostrando a mensagem ‘New File’.

[01:19] Então vamos começar a editar. Eu vou criar um novo servidor server {}.Vou colocar dentro das chaves o que eu quero com esse servidor agora, que é listen 80; ou seja, eu quero ouvir a porta 80, que é a porta padrão do HTTP.

[01:32] De novo, se na sua instalação aquele servidor padrão já veio na porta 80, então aqui você vai colocar ‘8080’ - porque isso depende muito do sistema operacional, da versão etc. Só para irmos alternando.

[01:48] Estou ouvindo a porta 80 aqui e eu vou colocar server_name localhost; porque eu estou acessando a minha própria máquina e eu não tenho nenhum outro nome para minha máquina, só ‘localhost’.

[02:00] Se eu não colocar nenhum server name eu posso acessar através do IP da minha máquina, ou ‘127001’ ou “000”, enfim - mas eu vou dar um nome aqui para continuarmos utilizando o ‘localhost’.

[02:12] Tenho o servidor configurado. Agora eu quero informar para ele: “quando chegar alguma requisição para qualquer lugar, você vai tentar buscar os arquivos em alguma pasta”.

[02:24] Quando chegar uma requisição em location /, ou seja, a partir de qualquer lugar e aqui nessa ‘location’ nós podemos fazer bastante coisas. Eu posso ter alguma rejects alguma coisa; eu posso dizer que quando chegar ‘/alguma-coisa’ e qualquer resto que vier, eu vou ter essa configuração aqui.

[02:46] Mas no meu caso, como eu coloco só barra (/), significa que qualquer requisição que chegar nesse servidor, vai entrar nesse bloco.

[02:55] Então fica aí uma dica, bem para o futuro que iremos trabalhar. Posteriormente eu poderia, por exemplo, ver se essa ‘location’ é um arquivo PHP. Se for um arquivo PHP, mando isso para outro lugar, para o servidor de aplicação. Mas enfim, vamos focar no que temos aqui.

[03:11] Sempre que chegar uma requisição para qualquer lugar, eu vou definir essas configurações. Qual vai ser a raiz dos arquivos desse meu servidor? Qual vai ser o root? Então, eu vou colocar isso como uma pasta do meu usuário. Você vai colocar uma pasta que você preferir. De novo, a recomendação é que não tenha nenhum caminho com espaços.

[03:32] Então no meu caso, eu vou adicionar nessa pasta root /Users/vinicius.dias/Dev/nginx;. Então aqui dentro eu vou ter os meus arquivos HTML, de imagem etc.

[03:45] Repare que eu estou usando o caminho completo. Já naquele servidor, naquele arquivo de configuração padrão, naquele include, ele tava passando um caminho relativo. Eu estou passando o caminho absoluto porque eu não quero meus arquivos aqui dentro da pasta ‘server’.

[04:01] Então eu vou botar em outro lugar completamente diferente, para isso eu coloco o caminho completo. Se você está no Linux o padrão também é desse mesmo formato. Se você está no Windows, sempre comece com ‘C:’ ou ‘D:’ - o nome do seu disco.

[04:16] Defini o root do meu servidor, mas e se eu acessar simplesmente ‘localhost’? Fizer isso sem colocar o nome de algum arquivo? Eu posso definir um arquivo padrão que vai ser carregado através do ‘index’.

[04:29] Então vou dar o nome deste arquivo de index.html, igual nós já tínhamos lá. Teoricamente configurado, agora se eu vir aqui no meu navegador e acessar https://localhost:80 ou eu posso até omitir esse ‘80’ (porque é a porta padrão HTTP), eu espero receber alguma coisa tentando acessar aquele arquivo, ou algo do tipo, um ‘404’ou algo assim.

[04:57] Mas eu não consegui acessar, ele está recusando a conexão. Por que será que isso está acontecendo? Será que nosso arquivo de configuração está errado? Será que faltou alguma coisa? Vamos entender um pouco melhor esse processo no próximo vídeo.



# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
#  03 Criando um servidor

- Criando um default novo para a aula 3
/home/fernando/cursos/nginx-alura/002-Servidor-HTTP/sites/default_v2.conf

Vamos configurar a porta 8080, pois a 80 já está em uso pelo nginx server default original

~~~~conf
server {
        listen 8080;
        server_name localhost;
}
~~~~



[04:16] Defini o root do meu servidor, mas e se eu acessar simplesmente ‘localhost’? Fizer isso sem colocar o nome de algum arquivo? Eu posso definir um arquivo padrão que vai ser carregado através do ‘index’.

[04:29] Então vou dar o nome deste arquivo de index.html, igual nós já tínhamos lá. Teoricamente configurado, agora se eu vir aqui no meu navegador e acessar https://localhost:80 ou eu posso até omitir esse ‘80’ (porque é a porta padrão HTTP), eu espero receber alguma coisa tentando acessar aquele arquivo, ou algo do tipo, um ‘404’ou algo assim.



~~~~bash
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled# vi default-2.conf
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled# cat default-2.conf
server {
        listen 8080;
        server_name localhost;

        location / {
            root /home/fernando/cursos/nginx-alura/002-Servidor-HTTP/arquivos-para-aula;
            index index.html;
        }
}
root@debian10x64:/etc/nginx/sites-enabled# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
root@debian10x64:/etc/nginx/sites-enabled# date
Mon 11 Sep 2023 12:04:23 AM -03
root@debian10x64:/etc/nginx/sites-enabled#
~~~~



http://192.168.0.110:8080/
<http://192.168.0.110:8080/>

403 Forbidden
nginx/1.14.2





- Criado arquivo html
arquivos-para-aula/index.html

- Novo teste

http://192.168.0.110:8080/
<http://192.168.0.110:8080/>

abriu:

PÃ¡gina de teste 2!

Se abrir, indica que o teste estÃ¡ OK.

For online documentation and support please refer to nginx.org.
Commercial support is available at nginx.com.

Thank you for using nginx.
