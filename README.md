# Project Overview:
Effectively and securely managing infrastructure is essential for modern DevOps workflows. This guide will create an End-to-End Infrastructure-as-Code (IaC) pipeline to provision an AWS EKS cluster using Terraform. We will automate the pipeline with Jenkins and securely manage sensitive credentials using HashiCorp Vault.

# High-Level Architecture:
1. Jenkins Server:
Deployed on an EC2 instance to manage CI/CD pipelines.
Jenkins plugins installed: Terraform, Pipeline, AWS Credentials, and HashiCorp Vault.

2. Vault Server:
Configured on a separate server to securely store and retrieve secrets (AWS and GitHub credentials).
Secrets stored:
aws/terraform-project: Contains aws_access_key_id and aws_secret_access_key.
secret/github: Contains GitHub PAT (pat).

3. Terraform Configurations:
Infrastructure as Code (IaC) to provision:
VPC, Subnets
EKS Cluster
Worker Nodes

4. CI/CD Pipeline:
Jenkins pipeline retrieves secrets from Vault and runs Terraform stages for EKS provisioning.

# Prerequisites:
1. AWS Account:
Ensure IAM permissions:
AmazonEKSFullAccess
AmazonEC2FullAccess
IAMFullAccess

2. EC2 Server for Jenkins:
Jenkins installed and configured with required plugins.
Vault credentials (vault-role-id, vault-secret-id, VAULT_URL) configured in Jenkins Credentials Store.
Preinstall kubectl on the Jenkins Server:

3. HashiCorp Vault Server:
Vault configured with secrets:
aws/terraform-project → AWS credentials.
secret/github → GitHub PAT.
AppRole authentication configured (vault-role-id and vault-secret-id).
Best Practices:
3.1 Automate Credential Renewal:

Since secret_id expires (the default TTL is 24 hours), consider automating its regeneration and updating Jenkins credentials programmatically.
Use Vault CLI or API scripts to refresh credentials periodically.
3.2 Secure the Jenkins Secrets:

Ensure Jenkins credentials are properly encrypted and masked.
Use Jenkins Secret Text credentials to avoid plain-text exposure in pipeline logs.
3.3 Test Before Deploying:

Always validate the new role_id and secret_id using the Vault CLI before updating Jenkins.
Please refer to the link below for complete information:
