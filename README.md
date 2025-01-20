# Projeto Final Programa de Bolsas UOL - Turma Out. 2024

### 📄 Projeto para simular a migração automatizada de um sistema para a nuvem utilizando ferramentas da AWS.

**Problema:** A empresa "Fast Engineering S/A" gostaria de uma solução para seu e-commerce, visto que a demanda de acessos e compras cresceu muito e a solução adotada até o momento não está atendendo as necessidades do empreendimento.

📌 Solução usada pela empresa até o atual momento:

- 01 servidor para Banco de Dados Mysql (500GB de dados, 10Gb de RAM, 3 Core CPU);
- 01 servidor para a aplicação utilizando REACT – frontend (5GB de dados, 2Gb de RAM, 1 Core CPU);
- 01 servidor de backend com 3 APIs, com o Nginx servindo de balanceador de carga e que armazena estáticos como fotos e links. (5GB de dados, 4Gb de RAM, 2 Core CPU); 

![image](https://github.com/user-attachments/assets/154f6483-7375-41b6-8529-3f714ce6ce26)


Usar o Amazon MGN permite migrar aplicações com interrupções mínimas de funcionamento para os clientes, tem como principal característica ajudar a simplificar, agilizar e reduzir os custos da migração de aplicações. Devemos instalar o WAS Replication Agent nos servidores, após isso, o Agent faz um handshake de autenticação no endpoint da API do AWS MGN que é criptografado com TLS 1.3
O Replication Agent deve ser instalado em cada servidor para a fim de começar a replicar os dados na sub-rede da área de preparação
![image](https://github.com/user-attachments/assets/f0435d05-7063-4569-bdd7-0cbb6b9c3180)

![image](https://github.com/user-attachments/assets/3d028d8a-a568-4a0f-941d-83ba2e370fe7)
![image](https://github.com/user-attachments/assets/2b43e288-ec29-406b-a3a7-cf5093f5fca8)
![image](https://github.com/user-attachments/assets/ed263a27-1508-4161-a0e6-300d3c0bdd94)
![image](https://github.com/user-attachments/assets/11254f46-a8f2-4c96-aa19-4defbfede3e5)




