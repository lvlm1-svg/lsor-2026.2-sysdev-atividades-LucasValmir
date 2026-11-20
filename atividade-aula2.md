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
