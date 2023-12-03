
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# Git

git status
git add .
git commit -m "Módulo 1 - Load Balancer. Aula 04 Algoritmos de balanceamento."
eval $(ssh-agent -s)
ssh-add /home/fernando/.ssh/chave-debian10-github
git push
git status



# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# 04 Algoritmos de balanceamento

## Transcrição

[00:00] Boas-vindas de volta. No último vídeo nós implementamos uma nova forma de um balanceamento de carga. Nós incrementamos nosso conhecimento de balanceamento de carga. Mas nós estudamos muita prática e não falamos tanto da teoria, então vamos entender a teoria por trás de algoritmos de balanceamento de carga.

[00:18] Por padrão quando configuramos aquele upstream e definimos um proxy pass para um upstream nós temos essa figura, essa é a implementação padrão de um load balancer com nginx. No último vídeo aprendemos a definir pesos, para chegar em algo próximo desse cenário, onde quando tenho várias requisições posso mandar mais requisições para um servidor do que para o outro.

[00:42] Essa ideia nós já entendemos na prática, mas, na teoria, esse algoritmo tem um nome, e é um nome importante de conhecermos, até porque esse mesmo algoritmo é utilizado em cenários fora de balanceamento de carga, então o nome desse algoritmo é round Robin, ele é utilizado inclusive em contextos de escalonamento de processos.

[01:02] Não vou entrar em detalhes sobre o que é escalonamento de processos, mas imagina que tenho vários processos para serem executados na mesma CPU, no mesmo núcleo da CPU, como o processador vai decidir qual processo executar por tanto tempo? A implementação de round Robin define um espaço de tempo onde o processo vai ser executado, depois interrompido.

[01:25] Esse mesmo espaço de tempo, esse mesmo período vai ser executado por outro processo, esse mesmo período vai ser entregue para outro processo executar. Depois outro, depois outro, e assim ele vai rodando. Quando o último finalizou eu volto para o primeiro. Essa é a ideia por trás de um round Robin.

[01:42] Quando tenho essa mesma ideia implementada em um load balancer tenho isso que o nginx traz por padrão. Se tenho três servidores, por exemplo, imagine que tenho mais um, vou fazer a ligação. Se tenho três servidores a primeira requisição chega nesse, a segunda no segundo, a terceira no terceiro.

[02:02] Acabou a lista de servidores nós voltamos para o começo, por isso esse nome round Robin, então vamos em quarta requisição, quinta requisição, sexta requisição. Esse é o algoritmo round Robin, só que quando precisamos definir pesos diferentes para cada um dos servidores, por exemplo, temos o que é chamado de waited round Robin ou round Robin ponderado.

[02:22] Então atingimos esse objetivo através da diretiva wait, demos um peso maior para o servidor, para o primeiro servidor, e o segundo servidor tem um peso um pouco menor. A primeira requisição chega aqui, a segunda também, a terceira chega aqui, a quarta chega aqui, a quinta também, a sexta chega aqui. Assim vamos balanceando, baseado nesse espaço que temos, no número de requisições, puramente. Número de requisições que chegam.

[02:52] Mas existem outros algoritmos que podem fazer sentido em outros casos, não vou citar todos, para não nos complicarmos muito, mas vou mostrar mais dois aqui. Imagine que vai ser uma requisição, uma conexão aberta. A primeira requisição chegou aqui, chegou a segunda requisição, e a terceira. A quarta chegou, a quinta chegou também, e a sexta chegou embaixo.

[03:22] Mas o que acontece? Essa primeira requisição era para uma coisa muito simples, então ela já foi encerada, a terceira também. E vamos receber mais uma requisição embaixo, repare que nesse momento estou mandando mais requisições para o meu servidor mais fraco do que para o servidor mais potente, porque minha única métrica é a quantidade de requisições que estão chegando, e não a quantidade de requisições que estão abertas, ou seja, o número de conexões que meu servidor tem abertas.

[03:55] Nesse cenário, quando tenho requisições que podem ter comportamentos diferentes, onde posso ter requisições muito demoradas e requisições muito simples precisamos balancear isso de forma mais inteligente. Vou apagar isso tudo e vamos pensar em outro cenário.

[04:11] Vou manter só uma requisição, chegou a primeira requisição. Quando chega a segunda, o load balancer sabendo daqueles pesos que tenho que ter o dobro de requisições vai ver qual servidor tem menos conexões baseado naquele peso. Esse servidor pode receber uma conexão a mais porque ele ainda não tem o dobro lá, vou receber mais uma requisição.

[04:33] Chegou a próxima requisição, ele vai ver, opa, esse servidor tem duas conexões ativas, e esse outro nenhuma. Vou mandar para cá. E agora nesse meio tempo uma das requisições já foi finalizada. Essa ainda está ativa. Então o load balancer vai mandar mais uma para cá, e de novo temos só duas conexões ativas, então ele pode mandar a próxima para baixo, ele pode mandar mais uma para cá, porque ainda não tem o dobro.

[05:04] Quando essa requisição morreu, a próxima requisição pode vir para cá, porque tenho três em cima e nenhuma aqui. Ele vai balanceando de forma mais inteligente, sem sobrecarregar nenhum dos dois. Talvez isso tenha ficado muito complexo no desenho, mas basicamente é com peso ou sem peso ele vai ver quais servidores têm menos conexões e mandar essa requisição para lá.

[05:25] Vamos pensar nisso sem peso, para ficar um pouco mais simples. Chegou uma requisição, vamos trazer para cá. Essa requisição chegou aqui, quando chega a próxima requisição o load balancer vai ver qual tem menos requisições, conexões ativas, ele vai mandar para baixo, e agora qual tem menos conexões ativas? Está igual. Então começa de novo, vai lá para o primeiro servidor.

[05:50] Nisso, qual tem menos requisições ativas? O segundo, mandou para cá. Porém, enquanto isso, esse servidor a primeira requisição já foi respondida, quando mando a próxima requisição ele pode mandar para cá de novo, balanceando melhor. E nessa meio tempo essa requisição, essa conexão foi encerrada, então posso mandar a próxima para cá. Ele vai balanceando de forma mais inteligente.

[06:15] Essa é a ideia por trás desse algoritmo de list connections, ou menor número de conexões. E para implementar ele é bastante simples. Basta eu informar no início do meu upstream essa diretiva least_comn, com isso ele vai ver qual o servidor, qual dos meus servers tem menos conexões ativas, abertas, e vai mandar requisição para lá, e ele considera o peso.

[06:42] Segundo essa informação, again with server wait taken into consideration, então de novo com peso dos servidores sendo considerado. Aqui já temos dois algoritmos, o round Robin, que é o padrão, que já vem implementado sem informarmos nada, e nós temos o de least connections, o que podemos configurar se tivermos essa diretiva least connections.

[07:07] Existem vários outros, só vou passar o olho, por exemplo, IP Hash, então baseado no hash do ip, ou seja, nas informações do ip que chegou à conexão posso mandar sempre esse mesmo ip para o mesmo servidor. Dessa forma consigo manter uma seção ativa, informações stateful lá, isso não é tão comum assim, mas pode ser bastante útil em alguns cenários.

[07:30] Tenho um generic hash, posso ter alguma informação gerando hash, então sempre que cair nessa url vai para esse servidor. Quando cair nessa outra url vai para outro servidor. Posso ter informações assim, também é menos comum. Posso ter com least time, então para cada requisição o nginx pode ver qual servidor está com a menor latência além de menor número de conexões, então esse já é um pouco mais complexo, ele é utilizado apenas no nginx plus, ou seja, na versão paga.

[08:05] Repare que esses outros não têm essa observação, então usamos na versão open source, e o último que pode ser simples de entender, mas que pode trazer complexidade é o de random, ou seja, cada requisição vai ser mandada para um servidor selecionado de forma aleatória.

[08:22] Mas além dessa ideia aleatória nós podemos trazer algumas configurações. Por exemplo, de todos os servidores, tenho oito servidores, seleciono aleatoriamente dois, e dentro desses dois podemos distribuir através do menor número de conexões, do least time, etc.

[08:40] Podemos configurar as coisas de formas muito complexas, mas te garanto que os mais comuns são o round Robin e o waited round Robin, e o least connections, quando usamos um desses dois se todas as nossas requisições vão para um servidor de aplicação ou se todas as nossas requisições só servem conteúdos estáticos, o round Robin é perfeito, nós não precisamos de nada além disso.

[09:04] Agora, se algumas conexões, se um dos meus servidores é maior que outro, tem mais recursos, então o waited round Robin já resolve o caso. Agora se tenho requisições, conexões que vão demorar para ser respondidas e outras que são muito rápidas, então o algoritmo de least connections pode ajudar bastante. Eu posso ter muitas conexões demoradas em um servidor e passo a mandar mais requisições para o outro, dessa forma temos um rebalanceamento um pouco mais esperto.

[09:33] Então se esse é um pouco mais esperto, por que eu utilizaria o round Robin? Porque ele exige menos do nosso load balancer, ele recebe a requisição e simplesmente vê, o último que mandei foi o servidor 2? Então, agora é 1. O último foi 1? Então, agora é 2. Já esse de least connections precisa armazenar quantas conexões estão abertas, ele tem um custo a mais. Depende do cenário onde você vai implementar cada um, mas essas são as características, prós e contras.