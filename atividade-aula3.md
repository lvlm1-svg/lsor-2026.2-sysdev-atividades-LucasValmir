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


## O Que Foi Realizado Nessa Aula
Execuções de linhas de comando para analisar informações básicas de rede da VM e do sistema, como endereço de IP, endereço MAC, DNS etc. Também foi possível inspecionar o comportamento de rotas e testar conectividade de camadas. Além de aprender diferenciar o modo NAT do modo Bridge.

## Comandos Usados
As linhas de comandos a seguir foram executadas no NAT Mode da VM para buscar algumas informações.
- ip addr show
- ip link
- ip route
- netplan status

Ainda sendo executados no mesmo ambiente, mas com algumas variáveis para acessar outras informações.
- ping -c 4 10.0.2.2
- ping -c 4 8.8.8.8
- ping -c 4 127.0.0.1
- ping -c 4 google.com
- ping -c 4 10.0.2.15






## Testes e Explicações

### ip addr show
![legenda](./imagens/aula3_passo1_ip-addr-show.png)  
O principal motivo de usar esse comando é descobrir o **endereço IP**. Essa informação pode ser encontrada logo em baixo de link/ether. Nesse caso, é possível ver que é **10.0.2.15**.   
Outra informação é a **mascara de sub-rede: /24**. Isso significa que 10.0.2 é fixo, facilitando a comunição em rede local. 

### ip link
![legenda](./imagens/aula3_passo1_ip-link.png)  
Mostra as placas e dispositivos de rede disponíveis, seus nomes e endereços MAC.   
Aqui temos 2 interfaces de redes: loopback, usado para realizar teste locais; e rede de Ethernet para acessar internet. Além disso, é possível visualizar o **endereço MAC da interface de Ethernet**. Está depois de link/ether: **08:00:27:1c:7d:6a**.

### ip route
![legenda](./imagens/aula3_passo1_ip-route.png)   
Com esse comando também é possível visualizar o IP da VM. Mas o que chama atenção é o **endereço do gateway padrão: 10.0.2.2**.

### netplan status
![legenda](./imagens/aula3_passo1_netplan-status.png)  
Esse comando mostra o resumo do estado da rede e os principais dados: IP, gateway e DNS.

### ping -c 4 127.0.0.1
![legenda](./imagens/aula3_passo2_127.0.0.1.png)  
Vamos começar a testar a conectividade de rede. Como primeiro teste: enviar pacotes para o próprio sistema da VM (Ubuntu Server).

### ping -c 4 10.0.2.15
![legenda](./imagens/aula3_passo2_IP-da-VM.png) 
Esse outro teste é quase parecido com o anterior, a diferença é que a placa de rede precisa está ativa para ter sucesso na resposta.

### ping -c 4 10.0.2.2
![legenda](./imagens/aula3_passo2_2.2.png)  
Já esse outro testa se o Ubuntu Sever consegue acessar o roteador virtual do VirtualBox, enviando pacotes para ele.

### ping -c 4 8.8.8.8
![legenda](./imagens/aula3_passo2_8.8.8.8.png)  
Esse testa se pacotes conseguem passar da rede do computador físico e serem enviados para a internet.

### ping -c 4 google.com  
![legenda](./imagens/aula3_passo2_google.png)  
Se os pacotes conseguirem chegar num sevidor (o google foi escolhido), significa que o teste foi bem sucedido.





## Atualizando, instalando e usando Traceroute

### sudo apt update
![legenda](./imagens/aula3_passo3_sudo-apt-update.png)  
O objetivo desse comando é atualizar onde o Sistema busca para instalar novos pacotes.

### sudo apt install -y traceroute
![legenda](./imagens/aula3_passo3_sudo-apt-install--y-traceroute.png)  
Esse instala o programa Traceroute. O programa serve para rastrear o caminho em que os pacotes percorrem até chegar no endereço de destino.

### traceroute 8.8.8.8
![legenda](./imagens/aula3_passo3_traceroute-8.8.8.8.png)  
Aqui envia pacotes de dados para um servidor do Google mais próximo. Foram 30 lugares para eles chegarem no destinho, começando no Gateway 10.0.2.2. 





## Alteração da ligação da placa para mais testes

### Poweroff e alterando as configurações
![legenda](./imagens/aula3_passo4_menu-virtualbox.png)  
Para podemos seguir com os teste é preciso mudar uma configuração no VirtualBox. Para isso é preciso desligar o Sistema Ubuntu. Para desligar só basta executar o comando "poweroff". Agora é ir para tela inicial do VirtualBox e apertar no ícone de configurações.

### Acessando configurações de Rede
![legenda](./imagens/aula3_passo4_placa-NATMode.png)  
Na aba Rede perceba que em "Ligado a" o valor está como NAT. O objetivo é usar o Modo Bridge. Para isso clique em cima de "NAT" e em seguida "Placa em modo Bridge". A próxima imagem mostra como deve ficar. Depois aperte em "Ok" e siga para os próximos testes.
![legenda](./imagens/aula3_passo4_placa-BridgeMode.png)  
![legenda](./imagens/aula3_passo4_copy_menu-virtualbox.png)  
Para continuar os teste clique na Seta Iniciar para iniciar o Sistema novamente.





## Comparação entre NAT Mode e Bridge Mode

### ip addr show 
![legenda](./imagens/aula3_passo4_ip-addr-show.png)  
Comparando com o resultado da placa ligada em NAT, é possível perceber que o IP mudou. **Agora o novo IP é 172.20.21.3**.

### ip route
![legenda](./imagens/aula3_passo4_ip-route.png)  
 Comparando com o resultado anterior, percebemos que **também mudou o endereço do gateway padrão: 172.20.20.1**.

 ### ping -c 4 8.8.8.8
 ![legenda](./imagens/aula3_passo4_ping--c-4-8.8.8.8.png)  
 Comparando, a gente ver que **nada mudou na saida**. Pode até não ter mudado nada aqui, mas nos testes anteriores houve uma grande mudança (endereço IP e Gateway Padrão). E na prática **o que muda é que no Bridge Mode o caminho para acessar internet é menor: VM -> Roteador de Rede Local -> Internet**. 





## Comparação com o Sistema Windows 

### ipconfig /all
![legenda](./imagens/aula3_passo5_Windows-infors.png)  
É possível comparar com o próprio Sistema que está rodando o VirtualBox. Nesse caso o Windows.  
Procurando nos programas internos irá encontrar o Windows PowerShell, uma interface para executar linhas de comandos. Se você digitar "ipconfing /all" e apertar enter, irá mostrar detalhes das configurações de Rede do Sistema.  
Como saida muitas informações. As que chamam atenção são: endereço IP, 172.20.20.222; e Gateway Padrão, 172.20.20.1 (igual do Ubuntu Server no modo Bridge). 





## Conclusão
Nessa aula foi realizado execuções de comando para analisar informações de rede (endereço de IP, endereço MAC, Gateway etc) no Sitema Ubuntu Server que estava sendo rodado no VirtualBox. Também foi possível inspecionar o comportamento de rotas e testar conectividade de camadas do Sistema.  
Além disso, essa aula apresentou como mudar e diferenciar o modo NAT do modo Bridge. Outra coisa que também foi possível foi ver as informações de rede do Sistema que estava rodando o VirtualBox, nesse caso o Windows. 