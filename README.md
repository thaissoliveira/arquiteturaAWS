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

### ✳️ Quais atividades são necessárias para a migração?

A migração inicial seguirá a estratégia Lift-and-Shift (As-Is), garantindo a transição rápida do sistema atual para a infraestrutura AWS sem alterações estruturais. O primeiro passo será a avaliação do ambiente atual, identificando dependências e recursos necessários para a transição.

A migração dos servidores será realizada utilizando o AWS Application Migration Service (MGN), que permite migrar aplicações com interrupções mínimas de funcionamento para os clientes, tem como principal característica ajudar a simplificar, agilizar e reduzir os custos da migração de aplicações. Devemos instalar o WAS Replication Agent nos servidores, após isso, o Agent faz um handshake de autenticação no endpoint da API do AWS MGN. O Replication Agent deve ser instalado em cada servidor para a fim de começar a replicar os dados na sub-rede da área de preparação. Durante todo o processo, ferramentas como AWS CloudTrail e CloudWatch serão utilizadas para monitoramento e auditoria. 

A partir disto, será configurada a infraestrutura AWS, incluindo a criação de uma VPC com sub-redes públicas e privadas, instâncias EC2 para hospedar as aplicações, um banco de dados gerenciado RDS Multi-AZ para garantir alta disponibilidade e um bucket S3 para armazenamento de objetos. 

### ✳️ Qual o diagrama da infraestrutura na AWS?

![projeto arquitetura aws-etapa 1 drawio](https://github.com/user-attachments/assets/07eb6515-b544-46cd-87ce-247b38e22fab)

### ✳️ Quais ferramentas são necessárias?

##### Recursos AWS utilizados e conceitos importantes:

- **VPC (Virtual Private Cloud):** Permite a criação de uma rede virtual isolada na AWS, onde são configuradas sub-redes públicas e privadas.

- **EC2 (Elastic Compute Cloud):** Proporciona instâncias de servidores na nuvem equivalentes ao ambiente on-premises atual.

- **RDS (Relational Database Service):** Serviço gerenciado de banco de dados relacional com Multi-AZ, proporciona alta disponibilidade e failover automático.

- **S3 (Simple Storage Service):** Armazena objetos (arquivos, backups, logs) de forma escalável e segura. Pode ser usado para armazenar dados de migração, logs de auditoria e backups.

- **IAM (Identity and Access Management):** Controla permissões e acesso a serviços AWS com base em usuários, grupos e funções.

- **AWS CloudWatch:** Serviço de monitoramento que coleta métricas de infraestrutura (CPU, memória, tráfego de rede), cria alertas e gera logs para acompanhar a performance dos serviços.

- **AWS MGN (Application Migration Service):** Automatiza a replicação dos servidores on-premises (frontend, backend) para instâncias EC2 na AWS, facilitando a migração sem grandes alterações no código.

- **AWS Replication Agent:** Agente responsável por enviar dados/arquivos do ambiente on-premises para a VPC de staging, onde o Replication Server processa e armazena em EBS temporariamente.

- **Replication Servers:** Servidores intermediários responsáveis por processar os dados recebidos do Replication Agent e armazená-los temporariamente no EBS, garantindo que a migração ocorra de forma contínua e segura.

- **AWS EBS (Elastic Block Store):** Fornece armazenamento persistente para instâncias EC2 e servidores de staging, garantindo que os dados permaneçam disponíveis mesmo após reinicializações.

- **AWS DMS (Database Migration Service):** Serviço para migração de bancos de dados, utilizado para migrar um banco MySQL on-premises para Amazon RDS (MySQL) com mínima interrupção no serviço.

- **Load Balancer:** Distribui requisições HTTP/HTTPS entre instâncias EC2 (frontend e backend), melhorando a disponibilidade e balanceando a carga de tráfego.

- **AWS Router 53:** Serviço de DNS gerenciado, permitindo rotear tráfego para diferentes instâncias EC2, Load Balancers ou serviços AWS, além de oferecer alta disponibilidade e baixa latência.

- **Amazon Cloud Front:** Serviço de CDN (Content Delivery Network) que acelera a entrega de conteúdos estáticos e dinâmicos ao distribuir cópias em locais estratégicos (Edge Locations), reduzindo a latência.

- **AWS WAF (Web Application Firewall):** Firewall para proteger aplicações contra ameaças como SQL Injection, Cross-Site Scripting (XSS) e ataques de negação de serviço (DDoS).

- **WAS SNS (Simple Notification Service):** Serviço de notificação e comunicação assíncrona. Pode ser utilizado para alertar administradores sobre eventos críticos, como falhas na migração ou alta carga no sistema.

- **AWS Secrets Manager:** Gerencia credenciais e segredos sensíveis (como senhas de banco de dados e chaves de API) de forma segura, evitando a exposição dessas informações no código-fonte.

### ✳️ Como serão garantidos os requisitos de Segurança?

Para garantir a segurança da infraestrutura, serão implementadas políticas de IAM, Security Groups e NACLs, além da habilitação de criptografia em trânsito e em repouso. A política de backup será gerenciada pelo AWS Backup, assegurando cópias automáticas de RDS, S3 e EC2. Após a migração, testes de validação serão conduzidos para garantir que todas as aplicações e serviços estejam operando conforme esperado.

### ✳️ Como será realizado o processo de Backup?

O processo de backup será realizado utilizando o AWS Backup, conforme representado na arquitetura da imagem. O AWS Backup será responsável por gerenciar automaticamente os backups das instâncias migradas e do banco de dados RDS.

Algumas das soluções escolhidas para garantir a segurança dos dados e informações do sistema:

- O RDS (serviço de banco de dados relacional da aws) usado na arquitetura tem suporte nativo para backups automáticos. É possível configurar backups automáticos diários e snapshots manuais para restaurações específicas.
- Os backups do RDS são armazenados no Amazon S3 de forma segura e podem ser retidos por períodos configuráveis.
- Uso do AWS Backup, que oferece uma solução centralizada para gerenciar backups de EC2, RDS, EBS, e S3.
- É feita a replicação entre regiões (multi-region replication) para aumentar a resiliência.
- A presença de balanceadores de carga (Load Balancers) e múltiplas instâncias em sub-redes públicas e privadas sugere um foco em alta disponibilidade, o que reduz a necessidade de restauração frequente em caso de falhas.

### ✳️ Qual o custo da infraestrutura na AWS (AWS Calculator)? 

- Custo de migração

IMAGEMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMM

- Custo mensal na etapa 1

IMAGEMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMM

# Etapa 2: Modernização/Kubernetes 

### ✳️ Quais atividades são necessárias para a migração?

## 1️⃣ Planejamento e Análise
- 🔹 Identificar os requisitos específicos da aplicação (frontend, backend e banco de dados).
- 🔹 Definir a arquitetura de **alta disponibilidade** e as **Zonas de Disponibilidade (AZs)**.
- 🔹 Mapear dependências externas, serviços integrados e fluxos de dados.
- 🔹 Planejar a estratégia de migração, minimizando impactos operacionais.

## 2️⃣ Preparação do Ambiente AWS
- 🔹 Criar a **VPC** com subnets **públicas e privadas**, garantindo isolamento adequado.
- 🔹 Configurar **grupos de segurança (Security Groups) e NACLs** para controle de acesso.
- 🔹 Provisionar o **Amazon EKS**, definindo **Node Groups** e escalabilidade automática (**Cluster Autoscaler**).
- 🔹 Criar e configurar o **Elastic Load Balancer (ELB)** para distribuição de tráfego eficiente.
- 🔹 Definir **IAM Roles** para controlar permissões de acesso aos recursos AWS.
- 🔹 Configurar o **Amazon RDS Multi-AZ** para garantir redundância do banco de dados.

## 3️⃣ Containerização e Deploy no EKS
- 🔹 **Containerizar aplicações** backend e frontend utilizando **Docker**.
- 🔹 Criar e enviar imagens para o **Amazon Elastic Container Registry (ECR)**.
- 🔹 Criar **manifests Kubernetes** para os deployments, services e configurações no **EKS**.
- 🔹 Implementar **ConfigMaps e Secrets** para variáveis sensíveis da aplicação.
- 🔹 Configurar **Horizontal Pod Autoscaler (HPA)** para ajuste dinâmico de cargas.

## 4️⃣ Banco de Dados e Migração de Dados
- 🔹 Configurar **Amazon RDS Multi-AZ** para armazenar os dados de forma resiliente.
- 🔹 Utilizar o **AWS Database Migration Service (DMS)** para migrar os dados.
- 🔹 Validar a **integridade e consistência dos dados** após a migração.

## 5️⃣ Testes e Validação
- 🔹 Validar **conectividade entre serviços** nas subnets.
- 🔹 Simular **cargas de tráfego** para testar o balanceamento de carga (ELB).
- 🔹 Testar a **resiliência da aplicação** em diferentes cenários de falha.
- 🔹 Garantir que **autenticação e permissões** funcionam corretamente.

## 6️⃣ Monitoramento, Segurança e Backup
- 🔹 Configurar **Amazon CloudWatch** para logs, métricas e alertas.
- 🔹 Implementar **AWS Backup** para backups automatizados do **RDS e S3**.
- 🔹 Configurar **AWS Secrets Manager** para gerenciamento seguro de credenciais.
- 🔹 Habilitar **AWS WAF** e **Shield** para proteção contra ataques DDoS e outras ameaças.
- 🔹 Monitorar **custos e consumo de recursos** para otimizar a infraestrutura.


### ✳️ Quais ferramentas são necessárias?

- Docker: Containerização dos serviços.
- Kubernetes (EKS): Orquestração dos containers.
- Elastic Load Balancer (ELB): Balanceamento de carga.
- AWS RDS: Banco de dados gerenciado.
- AWS DMS: Migração de banco de dados.
- S3: Armazenamento de arquivos e backups.
- IAM: Gerenciamento de permissões.
- CloudWatch: Monitoramento e logs.
- Secrets Manager: Armazenamento seguro de credenciais.
- Terraform: Infraestrutura como código.
- ECR: Repositório de imagens Docker.

### ✳️ Qual o diagrama da infraestrutura na AWS?

IMAGEMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMM

### ✳️ Como serão garantidos os requisitos de Segurança?

1) Controle de Acesso

- Uso de IAM Roles para limitar permissões.
- Autenticação segura via Secrets Manager.

2) Proteção contra Ataques

- AWS WAF para evitar ataques DDoS e SQL Injection.
- CloudFront para proteger contra acessos diretos.

3) Isolamento de Rede

- Subnets públicas para frontend e privadas para backend.
- Comunicação segura via Security Groups e NACLs.

4) Monitoramento e Auditoria

- CloudWatch Logs e AWS GuardDuty para detecção de ameaças.
- CloudTrail para auditoria de acessos.

### ✳️ Como será realizado o processo de Backup?

1) Banco de Dados

- AWS Backup configurado para RDS.
- Snapshots automáticos e retenção conforme política definida.

2) Armazenamento de Arquivos

- Uso de S3 para backup de dados estáticos.
- Versão habilitada no S3 para evitar perda de arquivos críticos.

3) Recuperação de Desastres

- Replicação entre AZs para alta disponibilidade.
- Testes regulares para garantir integridade dos backups.

### ✳️ Qual o custo da infraestrutura na AWS (AWS Calculator)? 

Custo mensal da etapa 2, com uma arquitetura mordenizada:

IMAGEMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMM

# Conclusão

- A migração será realizada de forma segura e rápida na ``Etapa 1``, garantindo a continuidade dos serviços no ambiente AWS.
- A modernização para Kubernetes na ``Etapa 2`` trará maior escalabilidade, eficiência e aderência às boas práticas de arquitetura em nuvem.
- As diretrizes para segurança e backup garantem a proteção dos dados e da infraestrutura em ambas as etapas.
