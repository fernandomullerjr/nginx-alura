
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# Git

git status
git add .
git commit -m "Módulo 1 - Conhecendo a ferramenta. Aula 04 Instalação."
eval $(ssh-agent -s)
ssh-add /home/fernando/.ssh/chave-debian10-github
git push
git status





# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# 04 Instalação

## Transcrição

[00:00] Vamos falar sobre como nós podemos instalar o NGINX na nossa máquina.

[00:06] Existem várias e várias formas de termos o NGINX rodando no nosso computador. Uma delas é, por exemplo, você utilizar containers. Então se você já conhece Docker, eu recomendo que você tente realizar essas etapas dentro de containers do Docker.

[00:21] Mas para não ter esse conhecimento como pré-requisito, eu vou instalar diretamente na minha máquina.

[00:26] Então se você utiliza Linux, você pode simplesmente executar, por exemplo, o comando sudo apt install nginx. Ou utilizando o gerenciador de pacotes da sua distribuição Linux ao invés de apt.

[00:40] Se você está no Mac, você pode utilizar o Homebrew para instalar, então você pode digitar o comando brew install nginx. Então um comando, NGINX instalado.

[00:51] Já para quem utiliza o Windows, como essa é uma ferramenta de gestão de servidores e Windows não é tão usado em servidores web, nós vamos ter um trabalho a mais.

[01:04] Vamos acessar o site https://www.nginx.org, que é o site oficial desse servidor web de código aberto. Você vai vir aqui no canto inferior direito em “download”. Aqui você tem, na mainline version a versão 1.19.9, que é a versão atual do momento da gravação desse curso.

[01:21] Você pode utilizar qualquer versão que comece com o número 1. Se no momento que você estiver assistindo for 2 ponto alguma coisa, então o ideal é você pegar uma versão um pouco mais antiga, para garantir que as configurações que estamos fazendo aqui continuem funcionando.

[01:35] Então você vai baixar esse arquivo que vai ser um ZIP, e você vai extrair esse ZIP em um caminho que seja de fácil acesso através do terminal. Idealmente um caminho que não tenha espaço no nome, porque o Windows costuma trazer bastantes problemas nesse cenário.

[01:50] Então, por exemplo: dentro do seu disco “C:” você cria uma pasta “Servidor” ou “Server”, como você quiser. Você vai extrair desse ZIP e dentro dessa pasta vai ter o executável do NGINX. Ali dentro estarão os arquivos de configuração também.

[02:09] Vamos falar bem mais sobre arquivo de configuração, mas feito esse processo de extração desse ZIP, você vai ter acesso ao comando NGINX desde que você esteja nessa pasta.

[02:21] Então imagine que você extraiu em “C:\Servidor\NGINX”. Então você vai acessar com “cd”, que é change directory, ou seja, mudar o diretório. Você vai acessar a pasta pelo comando cd C:\servidor\nginx.

[02:40] E acessada essa pasta, você vai digitar simplesmente nginx. Isso aqui é o suficiente para você inicializar o servidor. No meu caso ele deu um monte de erros, porque eu já tenho o servidor do NGINX rodando.

[02:53] Então repare que ele mostrou aqui uma porta, que é a porta que vem configurada por padrão. Nós vamos falar bastante sobre arquivo de configuração, mas por padrão o nosso servidor vai subir na “8080”.

[03:06] Tendo visto isso daqui, então digitei o comando nginx no Linux, no Mac ou no Windows - nós temos um servidor rodando. Então, o que eu vou fazer? Aqui no meu navegador eu vou digitar https://localhost:8080. Agora temos uma mensagem: “bem-vindo ao NGINX”, e dessa forma nós temos o nosso servidor web instalado.

[03:29] E o que aconteceu aqui para essa mensagem ser exibida? Temos um arquivo HTML que tem essa mensagem de bem-vindo e outras coisas; e nós fizemos uma requisição HTTP para a nossa própria máquina usando essa porta.

[03:44] Então o NGINX está ouvindo essa porta aqui e ele está configurado para responder com esse HTML, ou seja, o nosso servidor web já está funcionando.

[03:55] Claro, o que queremos fazer é colocar alguma aplicação real para ser exibida aqui, então não queremos ver somente a mensagem padrão do NGINX. Então a partir do próximo capítulo vamos começar a conhecer os arquivos de configuração do NGINX.





# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# 04 Instalação

- Instalando
sudo apt update
sudo apt install nginx


- Abrindo página local
http://192.168.0.110/


Welcome to nginx!

If you see this page, the nginx web server is successfully installed and working. Further configuration is required.

For online documentation and support please refer to nginx.org.
Commercial support is available at nginx.com.

Thank you for using nginx.