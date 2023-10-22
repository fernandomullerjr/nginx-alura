
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# Git

git status
git add .
git commit -m "Módulo 3 - Proxy Reverso. Aula 03 Configurando."
eval $(ssh-agent -s)
ssh-add /home/fernando/.ssh/chave-debian10-github
git push
git status



# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# 03 Configurando

## Transcrição
[00:00] Vamos finalmente configurar realmente o proxy reverso.

[00:05] O que eu quero fazer? Eu vou configurar daquela forma bem simples e que não vai ter muita utilidade agora, só para conhecermos a sintaxe, para entendermos a lógica. Depois falaremos de mais utilidades reais.

[00:17] O que eu quero fazer é - se você se lembra, nós temos dois servidores rodando: um que no meu computador está na porta 8080, que é aquele com as configurações padrão do NGINX e com a mensagem padrão; e outro que nós criamos na porta 80, na porta padrão do HTTP.

[00:32] O que eu quero fazer é: sempre que eu acessar esse servidor na porta 8080, eu quero que ele faça o proxy reverso para o outro servidor, da porta 80. Então se eu vier aqui, por exemplo, e acessar no navegador http://localhost:8080/teste.html, eu não quero ver essa mensagem de não encontrado, eu quero ver aquela mensagem do arquivo teste, ou seja, eu quero acessar aquele outro servidor.

[00:55] Então vamos voltar para a raiz. Como que podemos fazer isso? Vamos nos arquivos de configuração. Eu vou acessar o arquivo ‘nginx.conf’, que é onde está aquele nosso servidor padrão.

[01:10] Com isso, eu vou na parte de ‘server’ onde temos o nosso ‘root’, ou seja, aqui no nosso ‘location’ onde temos o nosso ‘root’ e o ‘index’. Eu posso tirar esses dois aqui, eu não vou utilizá-los. Eu não vou acessar diretamente algum arquivo, nem nada do tipo.

[01:24] Então o que eu quero fazer é: acessei o ‘location’, alguma coisa nesse servidor, ou seja, partindo de ‘/qualquer-coisa’. O que eu vou fazer é um proxy pass, ou seja, utilizando o conceito de proxy eu vou passar essa requisição para: proxy_pass http://localhost:80;.

[01:45] Só que, de novo - como ‘:80’ é a porta padrão HTTP eu posso remover o ‘:80’. Então, com isso eu tenho uma configuração de proxy reverso. É bastante simples, tudo o que eu preciso informar é a diretiva ‘proxy_pass’ e o destino para onde eu vou mandar essa requisição.

[02:03] Então, com isso eu vou salvar e sair. Vou verificar se eu não digitei nada errado. Aparentemente está tudo certo. E vou recarregar com o comando nginx. -s reload.

[02:14] Agora o que eu espero é: quando eu atualizar o navegador, eu quero ver o arquivo padrão, o ‘Arquivo index’. Então quando eu atualizo, aparece ‘Arquivo index’.

[02:25] Se eu tento acessar o http://localhost:8080/teste.html, eu estou no ‘Arquivo teste’.

[02:31] Se eu tento acessar alguma coisa que não existe, eu espero ver aquela mensagem de erro que configuramos no outro servidor. Escrito ‘Mensagem de erro’.

[02:38] Então, agora quando eu acesso um servidor, ele está fazendo proxy para um outro servidor, pegando a resposta e nos devolvendo.

[02:46] Então, veja como é simples configurarmos um proxy reverso usando o NGINX. Mas, como eu comentei, ainda não tem muita utilidade no que estamos fazendo aqui. Então vamos fazer um cenário um pouco mais útil.

[02:58] Vamos trabalhar com outro servidor fora do NGINX e quando chegar uma requisição para o NGINX, se ele atender à algumas regras, mandamos para esse outro servidor - um servidor de aplicação, por exemplo. Mas vamos fazer isso no próximo vídeo!

