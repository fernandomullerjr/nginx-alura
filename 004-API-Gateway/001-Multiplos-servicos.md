
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# Git

git status
git add .
git commit -m "Módulo 4 - API Gateway. Aula 01 Múltiplos serviços."
eval $(ssh-agent -s)
ssh-add /home/fernando/.ssh/chave-debian10-github
git push
git status



# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
#  01 Múltiplos serviços

## Transcrição

[00:00] E aí, pessoal? Boas-vindas de volta a mais um capítulo desse treinamento, onde estamos conhecendo bastante sobre a ferramenta NGINX, que é um servidor web que também serve como proxy reverso e algumas outras coisas ainda vamos ver nesse treinamento.

[00:13] No capítulo anterior falamos bastante sobre proxy reverso, nós aprendemos a configurar e vimos um caso de uso real. Agora vamos falar um pouco de um cenário diferente.

[00:24] Hoje em dia, principalmente, é muito falado sobre o conceito de arquitetura baseada em microsserviços. Então, só quero explicar super resumidamente o que é essa tal de arquitetura de microsserviços - até porque existem treinamentos específicos sobre esse assunto aqui na Alura. Então eu só vou pincelar para eu te mostrar o propósito desse capítulo e, o que vamos alcançar com esse capítulo.

[00:48] Uma aplicação padrão (o que nós chamamos de aplicação monolítica) é uma única aplicação onde a partir do navegador, por exemplo, a partir de um site nós acessamos toda a regra de negócios, que tudo acontece, que faz o acesso ao banco de dados, por exemplo, e que nós recebemos essas informações.

[01:09] Então imagine que o site da Alura é uma aplicação monolítica. Você digita “alura.com.br", essa interface vai chegar lá e vai bater na lógica de negócios. Aí essa lógica de negócios vai acessar o banco de dados para descobrir quais são os cursos cadastrados, ele vai acessar as promoções, ele vai acessar as notícias e os últimos cursos lançados. Isso tudo é processado e entregue para você.

[01:32] Então essa é a ideia por trás de uma aplicação comum, uma aplicação monolítica. Só que existe uma outra forma de modelar, que é utilizando a arquitetura de microsserviços.

[01:43] Então, imagine a parte logada da Alura, “cursos.alura.com.br”. Para você acessar essa parte você precisa ter uma assinatura válida e isso envolve a matrícula acadêmica, envolve a parte financeira, a parte de gamificação, onde você tem um jogador e tem pontos. Então tem muitos detalhes que não necessariamente precisam andar juntos.

[02:06] O que é muito comum nos dias de hoje é você ter uma aplicação que cuida da parte financeira, você ter uma outra aplicação que cuida da parte acadêmica, ter outra aplicação que cuida da parte de gamificação ou jogos e você ter outra aplicação que cuida da parte de streaming de vídeos.

[02:24] Então, dessa forma nós organizamos um sistema em várias pequenas aplicações ou serviços. Então daí vem o nome microsserviços.

[02:35] Só que existe um problema se pensarmos nessa abordagem somente como eu te expliquei. Imagine “cursos.alura.com.br” e imagine que o front-end (a página em si) precisa acessar os dados dos cursos que você está matriculado, precisa exibir para você a sua sua matrícula financeira etc., ou quando sua assinatura foi feita.

[02:58] Então, o front-end vai precisar saber qual é a API, qual é o servidor, qual é o serviço que lida com os detalhes financeiros; qual é o servidor, qual é o serviço, qual é o computador que lida com a parte acadêmica - e às vezes isso pode acabar mudando. Eu posso trocar de um servidor para outro, eu posso mover esse serviço.

[03:20] Além disso, às vezes um serviço roda em mais de um servidor, já é outro serviço em um só, então essa parte de roteamento pode ficar bastante difícil. Como saber qual URL acessar, por exemplo?

[03:33] Então, o que vamos fazer nesse capítulo é - usando o conceito de proxy reverso, usando o que aprendemos com proxy pass, o que eu quero fazer? Por exemplo, eu quero acessar a URL http://localhost:8080/ e quando eu digitar http://localhost:8080/servico1 eu quero acessar um servidor, um serviço, uma parte de uma aplicação.

[03:58] Quando eu digitar http://localhost:8080/servico2, mesmo que eu esteja fazendo a requisição para a mesma URL, para o mesmo servidor, eu quero acessar um outro servidor, uma outra aplicação.

[04:09] Então é isso que vamos implementar, é por isso que esse capítulo está depois da partir de proxy reverso. Porque embora o que iremos aplicar tenha um outro nome, diferente de proxy reverso, nós vamos utilizar os conhecimentos de proxy reverso para fazermos isso.

[04:23] Dessa forma, voltando para o exemplo da Alura, é como se pela interface ele só precisasse conhecer “api.alura.com.br”, por exemplo. Baseado no endpoint, baseado na URL desse servidor, esse nosso proxy reverso manda para algum serviço, manda para o serviço financeiro, para o serviço acadêmico, serviço de gamificação etc.

[04:48] Então é isso que vamos implementar nesse treinamento! No próximo vídeo nós vamos criar dois serviços pequenos e rotear entre eles. Fazer com que uma URL mande para um serviço e outra URL mande para outro serviço. Então te espero no próximo vídeo para botarmos a mão na massa!






