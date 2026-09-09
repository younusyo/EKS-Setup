# EKS-Setup
A comprehensive guide and configuration files for automating scalable EKS deployments.

Architecture Overview
This project automatically deploys and configures:

Amazon VPC:

3 Public Subnets (configured with kubernetes.io/role/elb for external Load Balancers).

3 Private Subnets (configured with kubernetes.io/role/internal-elb for internal Load Balancers).

Managed NAT Gateway for private node internet access.

Amazon EKS Cluster:

Kubernetes control plane version 1.34

Public endpoint access enabled for cluster administration via kubectl.

EKS Managed Node Group:

Auto Scaling group configured with t3.medium instances (ON_DEMAND capacity).

Worker nodes placed safely inside private subnets.

Prerequisites
Terraform >= 1.3.0

AWS CLI configured with valid IAM credentials

kubectl for interacting with the EKS cluster

Manual Deployment
1. Initialize Working Directory
Download required provider plugins and modules:

Bash
terraform init
2. Review Infrastructure Plan
Generate and inspect the execution plan:

Bash
terraform plan
3. Deploy Infrastructure
Apply the configuration to provision resources on AWS:

Bash
terraform apply -auto-approve
4. Connect to EKS Cluster
Once deployment completes, update your local kubeconfig:

Bash
aws eks update-kubeconfig --region $(terraform output -raw region) --name$(terraform output -raw cluster_name)

# Verify worker node connectivity
kubectl get nodes
