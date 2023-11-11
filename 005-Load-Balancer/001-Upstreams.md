
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
# 01 Upstreams

Transcrição
[00:00] E aí, pessoal, boas-vindas de volta a mais um capítulo desse treinamento onde estamos brincando com o NGINX, que é um servidor web que pode servir como proxy reverso - e como vamos ver neste capítulo, ele também pode servir como load balancer.

[00:14] Como comentei no capítulo anterior, pode acontecer de eu ter o NGINX e ele estar servindo como proxy reverso, só que o servidor final (o servidor de aplicação, por exemplo, ou outro servidor web) estar recebendo mais requisições do que ele consegue aguentar.

[00:31] Então está consumindo muitos recursos, as requisições estão se tornando mais lentas, então o que eu quero fazer é dividir essa carga. Agora eu quero deixar uma coisa clara: cada um desses servidores que criamos aqui é como se fosse um computador diferente.

[00:52] Então, aqui o “localhost: 8080” é como se fosse um computador que está servindo de proxy reverso, esse “8001” é como se fosse outro computador que tem um serviço e esse “8002” é como se fosse outro computador que tem esse outro microsserviço.

[01:09] Então eu estou criando vários serviços na mesma máquina para simplificar, para não precisarmos criar containers e para não precisarmos lidar com redes. Então é para nós simplificarmos e termos tudo na mesma máquina.

[01:18] Só que a ideia é, como eu disse: o “8001” é um computador respondendo à requisição, o “8002” aqui é como se fosse uma outra máquina respondendo essas requisições.

[01:31] Então, o que eu quero agora é: imagine que na máquina que eu tenho aqui em “localhost: 8002” e na máquina “localhost: 8001” eu quero ter a mesma aplicação. Por quê?

[01:45] Quando eu acessar aqui, por exemplo, outro serviço que eu vou criar aqui, quando eu acessar essa URL sem passar nada, eu quero que ora eu caia no “serviço 1” e ora eu caia no “serviço 2”. Ou seja, eu quero ir balanceando essa carga para não sobrecarregar nenhum dos dois servidores.

[02:03] Então eu vou dividir a minha aplicação em dois servidores e cada hora eu vou mandar a requisição para um dos dois, é esse o objetivo que eu quero atender.

[02:11] Então, vamos para o terminal usar o comando nginx -s reload, vamos ver se as configurações estão atualizadas. Se você acessar no navegador http://localhost:8003, dá o erro “ERR_CONNECTION_REFUSED”, porque não tenho esse servidor configurado.

[02:22] Então vamos configurar esse servidor como um load balancer, um balanceador de carga que ora manda para “8002” e ora manda para “8001”.

[02:33] No terminal, deixe-me inclusive renomear aquele mv Dev/nginx/servico2/servico.html para Dev/nginx/servico2/index.html. Então, agora “8001” é o “Serviço 1”, “8002” é o “Serviço 2” e quando eu acessar o “8003” eu quero cada hora cair em um desses dois servidores.

[02:54] De novo, imagine que eles têm a mesma aplicação, o mesmo código. Quando você faz o build você manda para esses dois servidores e o nosso load balancer vai cada hora mandar a requisição para um dos dois.

[03:06] Então, vamos nessa! No terminal eu vou criar vim /usr/local/etc/nginx/servers/load-balancer.conf. O nome você pode escolher, eu estou criando um novo servidor aqui.

[03:25] Então aqui eu vou ter o meu server {. Ele vai ouvir, então listen 8003;. O nome do servidor é server_name localhost; como já estamos habituados. Agora, quando ele acessar qualquer coisa (ou seja, um ‘location /’), eu preciso mandar ele para algum lugar proxy_pass http://. Então eu preciso definir alguma coisa que represente esse conjunto de servidores - e com o NGINX eu posso fazer isso através da diretiva upstream.

[04:02] Então aqui eu vou dar um nome para esse upstream, por exemplo - upstream serviços {. Aqui em upstream serviços eu posso definir alguns servidores. Eu posso definir outras coisas, mas principalmente, servidores.

[04:14] Então vou definir, por exemplo, server localhost:8001; e vou definir um outro servidor server localhost:8002;. Então quando aqui dentro do servidor que eu estou definindo, eu me referir ao servidor serviços, o NGINX vai saber: “É qualquer um desses dois aqui, então eu vou mandar ora para um e ora para outro”.

[04:41] Então, vamos lá! Eu posso simplesmente utilizar o nome 'serviços' location / { proxy_pass http://servicos;. Com isso, se eu não fiz nenhuma bobeira, nós vamos verificar com o comando nginx -t.

[04:49] Aparentemente a sintaxe está certa, então vamos usar o comando nginx -s reload. O que vai acontecer agora? Deixe-me abrir o servidor de novo. Quando eu acessar o “localhost:8003”, em qualquer URL, ele vai fazer o proxy reverso para algum dos servidores aqui dentro de serviços. Em upstream serviços eu tenho o ‘8001’ e o ‘8002’. Então, ele vai balancear a carga.

[05:14] Então para vermos isso acontecer, eu vou atualizar o navegador.

[05:17] Caiu no “Serviço 1”. Vou atualizar de novo.

[05:19] Agora no “Serviço 2”! Ora no “Serviço 2” e ora no “Serviço 1”, então ele vai balanceando. A cada requisição que chega ele vai mandar para um dos servidores. Dessa forma, balanceamos a carga.

[05:34] Existem algoritmos mega complexos de balanceamento de carga - round-robin etc. - mas a forma mais rudimentar de um load balancer é essa. Metade dos serviços vai para cada um dos servidores.

[05:46] Nós podemos posteriormente estudar outros algoritmos de balanceamento, mas essa é a primeira forma de fazermos isso.

[05:53] Só tem um detalhe um tanto quanto interessante e importante de vermos quando fazemos proxy reverso - seja com balanceamento de carga, seja com o API Gateway, ou seja para um proxy reverso para um servidor de aplicação - a parte de logs pode ficar um pouco confusa.

[06:10] Então no próximo vídeo,nós vamos aprender a ler os logs de um dos nossos servidores. Para isso, vamos ter que configurar algum log, e a partir desse log vamos encontrar um problema.

[06:22] Então vamos passo-a-passo aprendendo a lidar com essa parte de logs usando NGINX.





# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# 01 Upstreams


- Criando arquivo de conf, que vai efetuar o balanceamento entre 2 serviços:

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



- Ajustando:

~~~~bash
root@debian10x64:/etc/nginx/sites-enabled# vi load-balancer.conf
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

~~~~




- Testando:

~~~~bash

root@debian10x64:/etc/nginx/sites-enabled# curl http://192.168.0.110:8003/
Serviço 1
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled# curl http://192.168.0.110:8003/
Serviço 2
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled# curl http://192.168.0.110:8003/
Serviço 1
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled# curl http://192.168.0.110:8003/
Serviço 2
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled# curl http://192.168.0.110:8003/
Serviço 1
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled# curl http://192.168.0.110:8003/
Serviço 2
root@debian10x64:/etc/nginx/sites-enabled#
root@debian10x64:/etc/nginx/sites-enabled# date
Sat 11 Nov 2023 05:01:17 PM -03
root@debian10x64:/etc/nginx/sites-enabled#

~~~~