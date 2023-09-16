
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# Git

git status
git add .
git commit -m "Módulo 2 - Servidor HTTP. Aula 05 Sinais e comandos."
eval $(ssh-agent -s)
ssh-add /home/fernando/.ssh/chave-debian10-github
git push
git status



# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# 05 Sinais e comandos

## Transcrição

[00:00] Nós tentamos configurar o nosso servidor, mas teve um problema aqui de conexão recusada. Então vamos entender o que aconteceu.

[00:10] Quando eu inicializei o meu servidor web,o meu NGINX, ele leu aquele ‘nginx.conf’, aquele arquivo padrão, carregou tudo o que tinha para carregar e pronto, subiu o nosso servidor.

[00:21] Eu fui lá, eu criei um arquivo de configuração, só que o NGINX não está nem aí para mim porque ele já carregou todas as configurações que tinha que carregar. Então eu preciso informar: “olhe só, NGINX, eu mudei alguma coisa. Eu preciso que você recarregue as configurações”. Então, para isso vamos aprender mais um comando aqui do NGINX.

[00:42] Deixe-me limpar a tela e usar o comando nginx -h para nós vermos aquela ajuda. Com esse -s nós podemos informar sinais para o processo principal. Podemos parar o processo; podemos pedir para ele sair, nós podemos reabrir o processo ou, exatamente o que nós queremos, recarregar as configurações desses processos.

[01:05] Então tudo o que precisamos fazer aqui é digitar o comando nginx -s reload. Teoricamente está tudo certo, ele recarregou as configurações. Agora, se eu tentar acessar pelo navegador nós temos o “404 Not Found” - que é exatamente o que eu esperava, porque nós não criamos o arquivo onde ele está tentando buscar o nosso ‘root’.

[01:23] Então, o que nós tivemos aqui? Nós passamos um sinal para o serviço do NGINX. Nós podemos também fazer algumas outras coisas. Por exemplo: podemos tentar verificar se nossos arquivos de configuração estão corretos. Então se eu faço o comando nginx -t, ele verifica que a sintaxe está OK, então o teste deu tudo certo e o nosso arquivo de configuração está beleza.

[01:47] Se eu tentar acessar aquele nosso arquivo de configuração e tirar um ponto e vírgula, por exemplo, salvar e fazer o comando nginx -t - aí ele já nos avisa que esse arquivo tem um erro. Então vamos corrigir o erro e adicionar o ponto e vírgula. Então dessa forma conseguimos ver se está tudo OK com o arquivo e conseguimos recarregar as configurações.

[02:10] Vamos de novo dar uma olhada aqui no arquivo 'default.conf' e criar essa pasta dentro do meu usuário. Em ‘Dev’ eu quero criar uma pasta ‘nginx’ e ter o nosso arquivo ‘index.html’. Deixe-me sair daqui.

[02:25] Aqui, no terminal, eu já estou na pasta do meu usuário, então dentro de ‘Dev’ eu quero criar ‘nginx’. Agora dentro dessa pasta ‘nginx’, eu vou criar o nosso ‘index.html’, então vou simplesmente escrever aqui ‘Arquivo index’.

[02:39] Então você vai, na pasta onde você definiu como ‘root’, criar o arquivo ‘index.html’ e botar qualquer conteúdo que você quiser. Pode escrever um HTML válido, ou no meu caso aqui, como sou preguiçoso, só uma mensagem. Salvei e posso vir no navegador e carregar nosso arquivo “index”.

[02:55] “Vinicius, por que nesse cenário você não precisou recarregar as configurações?” Porque isso que eu fiz não foi mudar nenhuma configuração. Eu mudei um arquivo que o NGINX vai tentar acessar quando ele recebe a requisição. Então ele recebeu a requisição e agora o arquivo existe. Isso não é uma configuração.

[03:12] E se eu tiver um arquivo diferente de ‘index’, por exemplo? Se eu tiver aqui um ‘teste.html’, então escrevo ‘Arquivo teste’. Como será que eu posso acessar ele? Lá no nosso ‘location’ eu estou dizendo que barra (‘/’) vai acessar “index”.

[03:28] Só que na verdade, como eu já comentei, isso aqui significa que sempre que chegar uma requisição para barra e qualquer coisa mais para frente, ele vai tentar buscar aqui nesse ‘root’.

[03:38] Então se eu digitar aqui na barra de endereço do navegador, além do barra, o ‘teste.html’: https://localhost:80/teste.html ele vai acessar o nosso arquivo teste. Agora, se eu acessar alguma coisa que não existe - vou digitar ‘alguma-coisa’ - ele vai tentar encontrar esse arquivo ‘alguma-coisa’ e vai dar um “404”.

[03:54] Só que essa mensagem “404”, espero que você concorde comigo, não está muito bonita. Além de não ser a mais bonita possível, ela está nos trazendo informações do nosso servidor: "nginx/1.19.8".

[04:06] É interessante não expor esse tipo de informação para que atacantes não tenham esse tipo de informação, porque assim ele pode buscar as vulnerabilidades conhecidas nessa versão e tentar explorar isso.

[04:17] Então o que queremos fazer para o nosso site é criar uma página de “404” bonita ou uma página geral de erros mais bonita. Então vamos entender como podemos configurar páginas de erro usando o NGINX no próximo vídeo.



# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# 05 Sinais e comandos


nginx -s reload

  -s signal     : send signal to a master process: stop, quit, reopen, reload

O comando nginx -s reload é usado para recarregar a configuração do servidor web Nginx sem interromper o serviço. Especificamente:

    nginx: Este é o executável do servidor web Nginx.
    -s: Isso indica que você está fornecendo um sinal (signal) para o Nginx.
    reload: Este é o sinal que você está enviando para o Nginx, instruindo-o a recarregar sua configuração.

Quando você executa esse comando, o Nginx irá reler sua configuração, aplicar as alterações feitas nos arquivos de configuração (como nginx.conf) e recarregar o servidor para usar as novas configurações. Isso é útil quando você fez alterações na configuração do Nginx e deseja aplicá-las sem reiniciar o servidor, evitando assim qualquer interrupção no serviço.



nginx -t

O comando nginx -t é usado para testar a configuração do servidor web Nginx quanto a erros de sintaxe e configuração. Especificamente:

    nginx: Este é o executável do servidor web Nginx.
    -t: Isso indica que você deseja realizar um teste na configuração.

Quando você executa esse comando, o Nginx verifica sua configuração atual para verificar se há erros de sintaxe ou problemas de configuração. Ele não inicia ou recarrega o servidor, apenas verifica se a configuração está correta. Se a configuração estiver sem erros, você receberá uma mensagem indicando que a configuração está OK. Caso contrário, receberá uma mensagem de erro que descreve o problema encontrado.

Essa é uma maneira útil de verificar a integridade da configuração do Nginx antes de tentar recarregar o servidor, o que pode evitar problemas de inicialização do servidor devido a erros na configuração.






Erro 404