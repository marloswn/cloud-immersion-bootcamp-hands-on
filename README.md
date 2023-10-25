# Cloud immersion hands on
This project leverages a cutting-edge multicloud strategy, harnessing the power of both GCP and AWS, to deliver a state-of-the-art Infrastructure as Code (IAC) solution through the seamless integration of Terraform. It's a fictional scenario of a luxury hotel that needs to migrate its application and infrastructure to the cloud. This project is part of the Cloud Immersion bootcamp, hosted by <a href="https://thecloudbootcamp.com/" target="_blanck"> team.

## Required
- Both AWS and GCP accounts
- Create a terraform user on AWS IAM service, with "AmazonS3FullAccess" policy

## Stack
- IAC: Terraform
- Storage: AWS S3
- Database: Google Cloud SQL
- Application: hosted and processed on Google Kubernets Engine (GKE) using Google Container Registry (GCR)
- Change the bucket name in tcb_aws_storage.tf file to your desired bucket on S3

### Step 1: providing the infrastructure

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

### Step 2: preparing the SQL network
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
