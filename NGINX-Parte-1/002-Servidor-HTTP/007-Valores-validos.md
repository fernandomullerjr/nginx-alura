


# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# Git

git status
git add .
git commit -m "Módulo 2 - Servidor HTTP. Aula 07 Valores válidos."
eval $(ssh-agent -s)
ssh-add /home/fernando/.ssh/chave-debian10-github
git push
git status



# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# #####################################################################################################################################################
# 07 Valores válidos

Aprendemos a configurar páginas de erro neste vídeo através da diretiva error_page, informando códigos e um caminho.

Qual das alternativas a seguir é FALSA sobre configurações de páginas de erro no nginx?

Selecione uma alternativa

Podemos redirecionar diversos erros HTTP para o mesmo caminho


Precisamos informar pelo menos um código HTTP para definir a página de erro


Precisamos definir o caminho de um arquivo existente





## RESPOSTA

Precisamos definir o caminho de um arquivo existente


Alternativa correta! Essa afirmação é falsa pois no caminho informado ao error_page nós podemos ter um valor que corresponde a alguma diretiva location.

Parabéns, você acertou!
