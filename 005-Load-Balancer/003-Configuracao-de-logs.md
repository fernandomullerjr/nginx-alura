
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




root@debian10x64:/etc/nginx/sites-enabled# ls -lhasp /var/ | grep log
4.0K drwxr-xr-x 16 root root  4.0K Nov 11 00:00 log/
root@debian10x64:/etc/nginx/sites-enabled#
