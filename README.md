# EN-US

# Cloud immersion hands on
This project leverages a cutting-edge multicloud strategy, harnessing the power of both GCP and AWS, to deliver a state-of-the-art Infrastructure as Code (IAC) solution through the seamless integration of Terraform. This is a real-world scenario of a luxury hotel that needs to migrate its COVID guest testing application and infrastructure to the cloud. It's the implementation of the Cloud Immersion bootcamp, hosted by <a href="https://thecloudbootcamp.com/" target="_blank">The Cloud Bootcamp</a> team.

## Requirements
- AWS and GCP Accounts
- Create a user "terraform-pt-1" in the AWS IAM service, with programmatic access (CLI) and the "AmazonS3FullAccess" policy, create an access key for it, download, rename the CSV to "accessKeys.csv," and place it in the "mission1" directory.
- Create a user "luxxy-covid-testing-system-pt-app1" in the AWS IAM service, with programmatic access (CLI) and the "AmazonS3FullAccess" policy, create an access key for it, download, rename the CSV to "luxxy-covid-testing-system-pt-app1.csv," and place it in the "mission2" directory.
- Change the bucket name in the tcb_aws_storage.tf file (mission1 folder) to your desired S3 bucket.

## Stack
- IAC: Terraform
- Storage: AWS S3
- Database: Google Cloud SQL
- Application: hosted and processed on Google Kubernetes Engine (GKE) using Google Container Registry (GCR)
- Google Cloud Shell
- Google Cloud Editor

## Step 1: providing the infrastructure

Run the following commands on Google Cloud PowerShell:

#### Preparing the files
1) mkdir mission1_pt
2) mv mission1.zip mission1_pt (if you uploaded the unziped folder, just remove .zip in this command)
3) cd mission1_pt
4) unzip mission1.zip (if you uploaded the unziped folder, ignore this step)
5) mv ~/accessKeys.csv mission1/pt
6) cd mission1/pt
7) chmod +x *.sh

#### Preparing the environment
1) ./aws_set_credentials.sh accessKeys.csv
2) gcloud config set project <your-project-id>

#### Setting the project on GCP
1) ./gcp_set_project.sh

#### Enabling Kubernetes, Container Registry and Cloud SQL
1) gcloud services enable containerregistry.googleapis.com
2) gcloud services enable container.googleapis.com
3) gcloud services enable sqladmin.googleapis.com

#### Providing the infrasctructure
1) cd ~/mission1_pt/mission1/pt/terraform/
2) terraform init
3) terraform plan
4) terraform apply
5) Type "yes"

## Step 2: preparing the SQL network
- Select your Cloud SQL instance
- Select "Connections"
- Select the Networking tab and in Instance IP assignment and enable Private IP
- Under Associated Network, select “default.”
- Click on Set up connection
- Enable the Service Networking API (if prompted)
- Select the option "Use an automatically allocated IP range in your network"
- Select Continue > Create connection
- Go to Authorized Networks, and click "Add Network"
- In New Network, enter the following information:
  - Name: Public Access (For Testing Only)
  - Network: 0.0.0.0/0
  - Click Done
 
## Step 3: Converting the application to run in a multicloud environment
- Create a database user and password in the Google Cloud SQL service.
- Connect to Google Cloud Shell and execute the following commands:
  - cd ~
  - mkdir mission2_en
  - cd mission2_en
  - wget https://tcb-public-events.s3.amazonaws.com/icp/mission2.zip
  - unzip mission2.zip
- Connect to the SQL instance with the command: mysql --host=<your_public_ip_cloudsql> --port=3306 -u app -p
- Create the "records" table with the following commands:
  - use dbcovidtesting;
  - source ~/mission2_en/mission2/en/db/create_table.sql
  - show tables; (to validate the table creation)
  - exit;
- Enable Google Cloud Build with the command: gcloud services enable cloudbuild.googleapis.com
  
## Step 4: Build the previously configured Docker image and edit the Kubernetes YAML file
- Execute:
  - cd ~/mission2_en/mission2/en/app
  - gcloud builds submit --tag gcr.io/<PROJECT_ID>/luxxy-covid-testing-system-app-en
- Modify the luxxy-covid-testing-system.yaml file in the following sections:
  - image (insert your project_id)
  - AWS_BUCKET (insert the name of your already created bucket)
  - S3_ACCESS_KEY (insert according to the luxxy-covid-testing-system-en-app1.csv file)
  - S3_SECRET_ACCESS_KEY (insert according to the luxxy-covid-testing-system-en-app1.csv file)
  - DB_HOST_NAME (insert the private IP of your database instance)
    
## Step 5: Deploy the application on the Kubernetes cluster
- Access GKE through the console, select the "Connect > Run in Cloud Shell" option, and execute the command to authenticate Kubernetes.
- Perform the deployment with the following command:
  - cd ~/mission2_en/mission2/en/kubernetes
  - kubectl apply -f luxxy-covid-testing-system.yaml
- In the GKE panel, go to "Services and Ingress" and find the IP of the generated endpoint.
- The application is up and running!

## Step 6: Migration of Application Data and Files
- In Google Cloud Shell, execute the following commands:
  - cd ~
  - mkdir mission3_pt
  - cd mission3_pt
  - wget https://tcb-public-events.s3.amazonaws.com/icp/mission3.zip
  - unzip mission3.zip
  - Connect to the database instance: mysql --host=<public_ip_address> --port=3306 -u app -p
  - Import the exported data from the on-premises database:
    - use dbcovidtesting;
    - source ~/mission3_pt/mission3/pt/db/db_dump.sql
-In AWS CloudShell, download the files using the following commands:
  - mkdir mission3_pt
  - cd mission3_pt
  - wget https://tcb-public-events.s3.amazonaws.com/icp/mission3.zip
  - unzip mission3.zip
  - Synchronize the downloaded data with S3:
    - cd mission3/pt/pdf_files
    - aws s3 sync . s3://luxxy-covid-testing-system-pdf-pt-<your_bucket>
- Test the application in the "View Records" tab to verify if the data and files are available.

And there you go! Your online solution is up and running!
 
# PT-BR

# Imersão em Nuvem Prática
Este projeto utiliza uma estratégia multicloud de ponta, aproveitando o poder tanto do GCP quanto da AWS, para fornecer uma solução de Infraestrutura como Código (IAC) de última geração por meio da integração perfeita do Terraform. Trata-se de um cenário do mundo real de um hotel de luxo que precisa migrar sua aplicação de testes de Covid dos hóspedes e sua infraestrutura para a nuvem. É a implementação do bootcamp Cloud Immersion, hospedado pela equipe [The Cloud Bootcamp](https://thecloudbootcamp.com/).

## Requisitos
- Contas na AWS e GCP
- Criar um usuário "terraform-pt-1" no serviço AWS IAM, com acesso programático (cli) e com a política "AmazonS3FullAccess", criar uma chave de acesso ao mesmo, baixando, renomeando o CSV para "accessKeys.csv" e colocando-o dentro do diretório "mission1"
- Criar um usuário "luxxy-covid-testing-system-pt-app1" no serviço AWS IAM, com acesso programático (cli) e com a política "AmazonS3FullAccess", criar uma chave de acesso ao mesmo, baixando, renomeando o CSV para "luxxy-covid-testing-system-pt-app1.csv" e colocando-o dentro do diretório "mission2"
- Alterar o nome do bucket no arquivo tcb_aws_storage.tf (pasta mission1)para o seu bucket desejado no S3

## Tecnologiass
- IAC: Terraform
- Storage: AWS S3
- Database: Google Cloud SQL
- Application: hosted and processed on Google Kubernetes Engine (GKE) using Google Container Registry (GCR)
- Google Cloud Shell
- Google Cloud Editor

## Etapa 1: Fornecendo a Infraestrutura

Execute os seguintes comandos no Google Cloud PowerShell:

#### Preparando os arquivos
1) mkdir mission1_pt
2) mv mission1.zip mission1_pt (se você carregou a pasta descompactada, apenas remova .zip neste comando)
3) cd mission1_pt
4) unzip mission1.zip (se você carregou a pasta descompactada, ignore esta etapa)
5) mv ~/accessKeys.csv mission1/pt
6) cd mission1/pt
7) chmod +x *.sh

#### Preparando o ambiente
1) ./aws_set_credentials.sh accessKeys.csv
2) gcloud config set project <seu-id-de-projeto>

#### Definindo o projeto no GCP
1) ./gcp_set_project.sh

#### Habilitando Kubernetes, Container Registry e Cloud SQL
1) gcloud services enable containerregistry.googleapis.com
2) gcloud services enable container.googleapis.com
3) gcloud services enable sqladmin.googleapis.com

#### Fornecendo a Infraestrutura
1) cd ~/mission1_pt/mission1/pt/terraform/
2) terraform init
3) terraform plan
4) terraform apply
5) Digite "yes"

## Etapa 2: Preparando o ambiente SQL
- Selecione a sua instância Cloud SQL
- Selecione "Conexões"
- Acesse a guia "Networking" e em "Atribuição de IP da instância", ative o IP Privado
- Em "Rede associada", selecione "default"
- Clique em "Configurar conexão"
- Ative a API de Networking do Serviço (se solicitado)
- Selecione a opção "Usar um intervalo de IP alocado automaticamente na sua rede"
- Clique em Continuar > Criar conexão
- Acesse "Redes Autorizadas" e clique em "Adicionar Rede"
- Em "Nova Rede", insira as seguintes informações:
  - Nome: Acesso Público (Apenas para Testes)
  - Rede: 0.0.0.0/0
  - Clique em Concluído
 
## Etapa 3: Convertendo a aplicação para rodar em ambiente multicloud
- Criar um usuário e senha de banco de dados no serviço Google Cloud SQL
- Conectar-se ao Google Cloud Shell e executar os seguintes comandos:
  - cd ~
  - mkdir mission2_pt
  - cd mission2_pt
  - wget https://tcb-public-events.s3.amazonaws.com/icp/mission2.zip
  - unzip mission2.zip
- Conectar-se à instância SQL com o comando: mysql --host=<your_public_ip_cloudsql> --port=3306 -u app -p
- Criar a tabela "records" com os comandos:
  - use dbcovidtesting;
  - source ~/mission2_pt/mission2/pt/db/create_table.sql
  - show tables; (para validar a criação da tabela)
  - exit;
- Habilitar o Google Cloud Build com o comando: gcloud services enable cloudbuild.googleapis.com

## Etapa 4: Realizar o build da imagem Docker previamente configurada e editar o arquivo YAML do Kubernetes
- Executar:
  - cd ~/mission2_pt/mission2/pt/app
  - gcloud builds submit --tag gcr.io/<PROJECT_ID>/luxxy-covid-testing-system-app-pt
- Alterar o arquivo luxxy-covid-testing-system.yaml nos seguintes trechos:
  -  image (inserir o seu project_id)
  -  AWS_BUCKET (inserir o nome do seu bucket já criado)
  -  S3_ACCESS_KEY (inserir de acordo com o arquivo luxxy-covid-testing-system-pt-app1.csv)
  -  S3_SECRET_ACCESS_KEY (inserir de acordo com o arquivo luxxy-covid-testing-system-pt-app1.csv)
  -  DB_HOST_NAME (inserir o ip privado da sua instância de banco de dados)
 
## Etapa 5: Realizar o deploy da aplicação no cluster Kubernetes
- Acessar o GKE pelo console, selecionar a opção "Conectar > Executar no Cloud Shell" e executar o comando, a fim de autenticar o Kubernets
- Realizar o deploy através do seguinte comando:
  - cd ~/mission2_pt/mission2/pt/kubernetes
  - kubectl apply -f luxxy-covid-testing-system.yaml
- Ainda no painel do GKE, acesso "Serviços e entradas" e o ip do endpoint gerado
- A aplicação está no ar!

## Etapa 6: Migração dos dados e arquivos da aplicação
- No Google Cloud Shell, realizar os comandos:
  - cd ~
  - mkdir mission3_pt
  - cd mission3_pt
  - wget https://tcb-public-events.s3.amazonaws.com/icp/mission3.zip
  - unzip mission3.zip
  - Conectar-se à instância de banco de dados: mysql --host=<public_ip_address> --port=3306 -u app -p
  - Importar os dados exportados do banco de dados on-premisse: 
    - use dbcovidtesting;
    - source ~/mission3_pt/mission3/pt/db/db_dump.sql
- No AWS CloudShell, realizar o download dos arquivos através dos comandos:
  - mkdir mission3_pt
  - cd mission3_pt
  - wget https://tcb-public-events.s3.amazonaws.com/icp/mission3.zip
  - unzip mission3.zip
  - Sincronizar os dados baixados com o S3:
    - cd mission3/pt/pdf_files
    - aws s3 sync . s3://luxxy-covid-testing-system-pdf-pt-<your_bucket>
- Realizar o teste na aplicação na aba "Ver registros" verificando se os dados e arquivos estão disponíveis.

E pronto! Solução online e rodando!
