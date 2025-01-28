# Projeto Final Programa de Bolsas UOL - Turma Out. 2024

### 📄 Projeto para simular a migração automatizada de um sistema para a nuvem utilizando ferramentas da AWS.

**Problema:** A empresa "Fast Engineering S/A" gostaria de uma solução para seu e-commerce, visto que a demanda de acessos e compras cresceu muito e a solução adotada até o momento não está atendendo as necessidades do empreendimento.

📌 Solução usada pela empresa até o atual momento:

- 01 servidor para Banco de Dados Mysql (500GB de dados, 10Gb de RAM, 3 Core CPU);
- 01 servidor para a aplicação utilizando REACT – frontend (5GB de dados, 2Gb de RAM, 1 Core CPU);
- 01 servidor de backend com 3 APIs, com o Nginx servindo de balanceador de carga e que armazena estáticos como fotos e links. (5GB de dados, 4Gb de RAM, 2 Core CPU); 

![image](https://github.com/user-attachments/assets/154f6483-7375-41b6-8529-3f714ce6ce26)

### 💡 Solução desejada:

A empresa quer modernizar esse sistema para a AWS, seguindo as melhores práticas arquitetura em Cloud AWS, a nova arquitetura deve seguir as seguintes diretrizes:

-  Ambiente Kubernetes;
-  Banco de dados gerenciado (PaaS e Multi AZ);
-  Backup de dados;
-  Sistema para persistência de objetos (imagens, vídeos etc.);
-  Segurança;

### Passos a serem seguidos para realizar a migração do sistema:

1) Antes da migração acontecer para a nova estrutura, precisamos fazer uma migração “lift-and-shift” ou “as-is”.
2) Promover a modificação para a nova estrutura em Kubernetes.

# Etapa 1: Migração As-Is

- Quais atividades são necessárias para a migração?
- Quais as ferramentas vão ser utilizadas?
- Qual o diagrama da infraestrutura na AWS?
- Como serão garantidos os requisitos de Segurança?
- Como será realizado o processo de Backup?
- Qual o custo da infraestrutura na AWS (AWS Calculator)? 

# Etapa 2: Modernização/Kubernetes 

- Quais atividades são necessárias para a migração?
- Quais as ferramentas vão ser utilizadas?
- Qual o diagrama da infraestrutura na AWS?
- Como serão garantidos os requisitos de Segurança?
- Como será realizado o processo de Backup?
- Qual o custo da infraestrutura na AWS (AWS Calculator)? 

# Conclusão

- A migração será realizada de forma segura e rápida na ``Etapa 1``, garantindo a continuidade dos serviços no ambiente AWS.
- A modernização para Kubernetes na ``Etapa 2`` trará maior escalabilidade, eficiência e aderência às boas práticas de arquitetura em nuvem.
- As diretrizes para segurança e backup garantem a proteção dos dados e da infraestrutura em ambas as etapas.


A validação de dados é uma opção que você pode adicionar à sua tarefa de replicação. A validação de dados acompanha o andamento da migração e valida incrementalmente novos dados à medida que são gravados no destino. 

1) Inicialmente, o sistema interagirá com o banco de dados original. Use o AWS DMS para executar uma migração de carga total e, em seguida, configure a replicação contínua.
2) Depois de verificar se os dados estão fluindo corretamente e supondo que você tenha testado a aplicação com o novo banco de dados, você mudará a aplicação para interagir com o novo banco de dados. Isso interromperá a replicação contínua.
3) 
Usar o Amazon MGN permite migrar aplicações com interrupções mínimas de funcionamento para os clientes, tem como principal característica ajudar a simplificar, agilizar e reduzir os custos da migração de aplicações. Devemos instalar o WAS Replication Agent nos servidores, após isso, o Agent faz um handshake de autenticação no endpoint da API do AWS MGN que é criptografado com TLS 1.3
O Replication Agent deve ser instalado em cada servidor para a fim de começar a replicar os dados na sub-rede da área de preparação
![image](https://github.com/user-attachments/assets/f0435d05-7063-4569-bdd7-0cbb6b9c3180)

![image](https://github.com/user-attachments/assets/3d028d8a-a568-4a0f-941d-83ba2e370fe7)
![image](https://github.com/user-attachments/assets/2b43e288-ec29-406b-a3a7-cf5093f5fca8)
![image](https://github.com/user-attachments/assets/ed263a27-1508-4161-a0e6-300d3c0bdd94)
![image](https://github.com/user-attachments/assets/11254f46-a8f2-4c96-aa19-4defbfede3e5)
![image](https://github.com/user-attachments/assets/1f728b9f-8dbf-4fb5-b7a0-4c61bede4bf1)
![image](https://github.com/user-attachments/assets/c984f84a-2b37-42b3-bc6f-f2dff44ff348)






