
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# Git

git status
git add .
git commit -m "Módulo 3 - Proxy Reverso. Aula 05 Necessidade real."
eval $(ssh-agent -s)
ssh-add /home/fernando/.ssh/chave-debian10-github
git push
git status



# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
#  05 Necessidade real

Neste vídeo nós fizemos a aplicação mais comum de um proxy reverso. Fazer com que algumas requisições sejam enviadas para um servidor de aplicação.

Por que realizar um proxy reverso e não apenas deixar o servidor de aplicação lidar com todas as requisições?

Selecione uma alternativa

Com nginx na frente podemos responder arquivos estáticos muito mais rapidamente.


Com nginx na frente nosso servidor de aplicação pode ficar offline às vezes sem problemas.


Não há nenhum motivo real para realizar esta tarefa.





# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
#  RESPOSTA

Com nginx na frente podemos responder arquivos estáticos muito mais rapidamente.


Alternativa correta! Esse é um dos principais motivos. O nginx é um servidor incrivelmente performático, então nós ganhamos muito ao não enviar todas as requisições para o servidor de aplicação. O nginx pode enviar diretamente os arquivos estáticos sem processar nada, além de poder definir cache, compressão, etc.