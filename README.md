# EN-US

# Cloud immersion hands on
This project leverages a cutting-edge multicloud strategy, harnessing the power of both GCP and AWS, to deliver a state-of-the-art Infrastructure as Code (IAC) solution through the seamless integration of Terraform. It's a real world scenario of a luxury hotel that needs to migrate its application and infrastructure to the cloud. It's the implementation of the Cloud Immersion bootcamp, hosted by <a href="https://thecloudbootcamp.com/" target="_blank">The Cloud Bootcamp</a> team.

## Required
- Both AWS and GCP accounts
- Create a terraform user on AWS IAM service, with "AmazonS3FullAccess" policy
- Create and download the Access Key CSV file for this user and place it in the same folder as this project

## Stack
- IAC: Terraform
- Storage: AWS S3
- Database: Google Cloud SQL
- Application: hosted and processed on Google Kubernets Engine (GKE) using Google Container Registry (GCR)
- Change the bucket name in tcb_aws_storage.tf file to your desired bucket on S3

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
 
# PT-BR

# Imersão em Nuvem Prática
Este projeto aproveita uma estratégia multicloud de ponta, aproveitando o poder tanto do GCP quanto da AWS, para fornecer uma solução de Infraestrutura como Código (IAC) de última geração por meio da integração perfeita do Terraform. Trata-se de um cenário do mundo real de um hotel de luxo que precisa migrar sua aplicação e infraestrutura para a nuvem. É a implementação do bootcamp Cloud Immersion, hospedado pela equipe [The Cloud Bootcamp](https://thecloudbootcamp.com/).

## Requisitos
- Contas na AWS e GCP
- Crie um usuário Terraform no serviço AWS IAM com a política "AmazonS3FullAccess"
- Crie e faça o download do arquivo CSV da Chave de Acesso para este usuário e coloque-o na mesma pasta deste projeto

## Tencologias
- IAC: Terraform
- Armazenamento: AWS S3
- Banco de Dados: Google Cloud SQL
- Aplicação: hospedada e processada no Google Kubernetes Engine (GKE) usando o Google Container Registry (GCR)
- Altere o nome do bucket no arquivo tcb_aws_storage.tf para o seu bucket desejado no S3

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

## Etapa 2: Preparando a Rede SQL
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
