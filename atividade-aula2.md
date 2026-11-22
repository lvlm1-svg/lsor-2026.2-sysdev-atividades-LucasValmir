# Relatório

## Identificação
**Nome:** Lucas Valmir Lopes de Melos  
**Matrícula:** 2024333016  
**Turma:** 913A  
**Data:** 26/10/2026  
**Título:** Laboratório de Sistemas Operacionais e Redes (LSOR)

---


## Informações Gerais do Laboratório

Para manter a padronização das práticas, a compatibilidade de redes e a segurança nos testes, adotamos as seguintes diretrizes de ambiente de hardware e credenciais:

*   **Hipervisor:** [VirtualBox](https://www.virtualbox.org/)
*   **Sistema Operacional Guest:** Ubuntu Server 26.04 LTS (64-bit) .
*   **Hardware Padrão da VM:** 2048 MB de Memória RAM, 2 CPU Virtual e 32 GB de Disco Rígido.


---


## Nessa aula o que foi realizado
Apresentados conceitos de administração de usuários, grupos e acessos de diretórios no Ubuntu-Server. Além de ações práticas para memorização do conteúdo.







## Teorias Fundamentais
Sistemas Linux a segurança e integridade são bases para um modelo de propriedade e privilégios. Arquivos ou diretórios possue:
- **Usuário Dono** (-u): normalmente quem cria o arquivo.
- **Grupo Associado** (-g): Conjunto de usuários que possuem características semelhanes.
- **Outros** (-o): Qualquer outro usuário que não seja dono ou esteja num grupo associado a algo.  

Os 3 itens apresentados podem receber permissões:
- **Leitura** (Read -r): Permite ser ler e ser listado.
- **Escrita** (Write -w): Permissão de criar, modificar ou apagar.
- **Execução** (Execute -x): Permite exeutar algo (programa, script etc).






## Criação de Usuários
Vamos criar um cenário: uma infraestrutura de desenvolvedores que usam o mesmo sistema. Para que isso seja possível, é preciso que o sistema suporte mais de um usuário. O ubuntu-server permite isso. Só basta os desenvolvedores terem conhecimentos de administrar nessa área.

### Fulano
Para criar uma conta de usuário de forma simples é usar o comando **adduser**. Assim um diretório será criado para armazenar as coisas desse indivíduo e um próprio interpretador de comandos.  
![legenda](./imagens/aula2_passo1_criacao-fulado.png)  
Perceba que antes do comando principal existe o sudo. Ele é usado para que o comando seguinte tenha privilégios de administrador. E por último é o nome do novo usuário: fulano.

### Outras contas
Como normalmente uma infraestrutura tem vários funcionários, é preciso aumentar a quantidade de usuários no sistema. Então vamos adicionar mais contas: cicrano, belcrano e novato.

### Conferir contas criadas
Existe um meio para analisar as contas dos usuários que foram criadas no sistema. Isso é usando o tail, um programa para mostrar últimas linhas de um arquivo. O diretório do arquivo é: "/etc/passwd".   
![legenda](./imagens/aula2_passo1_contas-conferidas.png)






## Grupos dos Desenvolvedores
Para reunir e gerenciar permissões de forma escalável, é interessante criar um grupo de uni a equipe de desenvolvimento.

### Criação de um grupo
O comando é groupadd. Como ele precisa de privilégios de administrador para funcionar, sudo vem no começo da linha. No final da linha é o nome do grupo. Essa é a formula para criar um grupo chamado devs.  
![legenda](./imagens/aula2_passo2_groupadd-devs.png)  


### Comando de adicionar e sua estrutura
usermod é quem vamos usar. Ele também precisa de privilégios, então o sudo é preciso no início. Em seguida vem as flags de "adicionar" (a) e "grupo" (G). E no final o nome do grupo e quem será inserido nele.  
Estrutura: sudo usermod -aG <GroupName> <UserName>  
![legenda](./imagens/aula2_passo2_usermod-devs.png)  
O que a gente fez aqui foi adicionar o fulano, belcrano e cicrano no grupo devs. Deixamos o novato de fora para que ele represente perfil externo (Outros).  

### Conferir indivíduos no grupo
É possível validar se os usuários que colocamos em devs estão lá mesmo. Para esse fim, o comando grep. Ele é usado para buscar palavras-chaves em um arquivo.  
Veja em baixo como conferir o resultado:  
![legenda](./imagens/aula2_passo2_devs-conferidos.png)  






## Diretório Compartilhado
Agora vamos criar uma pasta no diretório /srv/projeto para todos os desenvolvedores.  

### Criação
![legenda](./imagens/aula2_passo3_mkdir-projeto.png)  
O comando principal (mkdir), foi usado como usuário root (sudo) e com a flag -p (usada para evitar erros caso o diretório já exista).  

### Análise
Vamos agora analisar as permissões desse diretório utilizando o comando `ls -ld`:  
![legenda](./imagens/aula2_passo3_ls-projeto.png)  
O primeiro caractere (d) indica que é um diretório.  
E se a gente dividir em 3 grupos de 3 caracteres essa sequência "rwxr-xr-x", temos "rwx", "r-x" e "r-x".  
O primeiro indica que o usuário root tem permissão de ler (r), escrever (w) e executar/navegar (x) o diretório. O segundo indica que grupo associado (root) tem permissão de leitura e navegação. Já o terceiro diz que os demais usuários (Outros) tem permissão de navegação e leitura.  





## Mudança de Dono e de Grupoo Associado
O que vamos fazer agora é tornar o diretório /srv/projeto ser administrado pelo usuário administrador e manipulado pela equipe de desenvolvimento (grupo devs).

### Alteração de Dono
Para mudar, o comando usando é `chown`:  
![legenda](./imagens/aula2_passo4_chown-administrador-projeto.png)  

### Mudança de Grupo
Para alterar, é usado o comando `chgrp`:  
![legenda](./imagens/aula2_passo4_chgrp-devs-projeto.png)  

### Validação do Resultado
Usando o mesmo comando para analisar as permissões do diretório...  
![legenda](./imagens/aula2_passo4_ls-projeto.png)  
Percebemos que a sequência não mudou, o que mudou foi o que vinha depois. Antes era "root" e "root". Agora é "administrador" e "devs". Isso está Ok.







## Política de Segurança no Trabalho
Todos se reuniram e chegaram na seguine conclussão: 
- O Dono (administrador) e o Grupo de Devenvolvimento (devs) devem ter controle total: leitura, escrita e navegação na pasta projeto.
- Outros estão proibidos de fazer qualquer ação na pasta.

### Aplicando Política no Sistema
Para usar as regras apresentadas é preciso passar para notação numérica e usar o comando `chmod` sobre o diretório da pasta.  
Abaixo é apresentada a tabela de notação numérica:
- `-` -> 0
- `r` -> 4
- `w` -> 2
- `x` -> 1  

Para transformar uma sequência em notação numérica, só basta somar os valores.  
O Dono precisa ter `rwx`. Então: 4 + 2 + 1, resultando 7. O mesmo se repete para o Grupo devs. Já em Outros, onde o novato está inserido, a soma resultará em 0 (`---`).  
Agora é usar o comando no modo root, com a númeração 770 e com o diretório projeto.  
![legenda](./imagens/aula2_passo5_política-770.png)  

### Análise do Resultado
Usando o mesmo comando para analisar as permissões do diretório...  
![legenda](./imagens/aula2_passo5_ls-projeto.png)  
Percebemos que, ao divir novamente os 3 grupos, temos agora: "rwx", "rwx" e "---". Isso indica que o administrador (Dono) tem permissão para ler, escrever e navegar no diretório. O mesmo para o Grupo devs. Já o terceiro indica que aqueles que estão inseridos em Outros, não tem permissões.


