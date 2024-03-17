# DevOps Infrastructure Management with Terraform and Terragrunt

## Overview

This project utilizes Terraform and Terragrunt for managing infrastructure as code (IaC) across multiple environments in AWS. The setup focuses on robustness, scalability, and team collaboration, emphasizing best practices in cloud infrastructure management.

### Key Components

- **AWS EKS**: Managed Kubernetes service for running containerized applications.
- **VPCs and Networking**: Custom network configurations for resource isolation and security.
- **S3 and DynamoDB**: For state management and lock mechanisms to prevent conflicts during concurrent operations.
- **IAM Roles and Policies**: Secure access control to AWS resources.
- **Kubernetes Add-ons**: Additional components such as the Cluster Autoscaler for managing resources efficiently.

## Challenges Encountered

Work on this project spanned from Saturday evening, 16 MAR 24, 18:00, with breaks for rest, focusing on several key areas:

- **State Management**: Transitioning to using an S3 bucket and DynamoDB table for Terraform state to enhance team collaboration.
- **Module Complexity**: Identifying issues with large, complex modules that are hard to manage, update, and test.
- **Security and Networking**: Stressing on secure VPC structure and Kubernetes configurations.
- **Documentation and Best Practices**: Following comprehensive guidelines from AWS, Terraform, Terragrunt, and Kubernetes.

### Encountered Issues:

1. **Variable Management**: Faced issues with correctly providing and managing variables across modules and environments.
2. **Debugging Effort**: The effort to debug and correct the configurations was significant, with uncertainty regarding success.
3. **Provider Configuration**: Challenges in properly defining and aliasing provider configurations for multi-region deployments.

## Solution Simplification for Non-Production

- **Decoupling**: Break down large infrastructure modules into smaller, manageable pieces.
- **Terragrunt for DRY Configurations**: Utilize Terragrunt to keep Terraform configurations DRY and maintainable.
- **Public Access for EKS**: Configuration allowing public access to EKS clusters for deployment from external systems.

### Git Repository Strategy

For production readiness, a structured approach was created, encapsulating:

- Separate directories for live infrastructure and modules (`infrastructure-live` and `infrastructure-modules`).
- Environment-specific configurations under `dev` and `staging` folders.
- Isolated components like `eks`, `vpc`, and `kubernetes-addons` for modular management.

## Running the Project

The infrastructure setup can be applied using Terragrunt commands within the project folder:

```bash
terragrunt init
terragrunt run-all apply
```

To interact with the deployed EKS clusters:

```bash
aws eks update-kubeconfig --name dev-solitics-demo --region eu-west-2
aws eks update-kubeconfig --name staging-solitics-demo --region eu-west-2
```

### Conclusion

This endeavor provided a profound learning experience in managing complex cloud infrastructures using Terraform and Terragrunt. Building the project from scratch is planned, to incorporate lessons learned and establish a foundation for a more streamlined and efficient infrastructure management process.


## Project Directory Structure
This section outlines the organization of the project's directories and files, detailing the setup for infrastructure management with Terraform and Terragrunt, as well as Kubernetes deployments.

.
├── infrastructure-live               # Live infrastructure configurations
│   ├── backend.tf                    # Backend configuration for Terraform state
│   ├── dev                           # Development environment configurations
│   │   ├── eks                       # EKS specific configurations for development
│   │   │   └── terragrunt.hcl        # Terragrunt configuration for EKS
│   │   ├── env.hcl                   # Environment specific variables for development
│   │   ├── kubernetes-addons         # Kubernetes add-ons for development
│   │   │   └── terragrunt.hcl        # Terragrunt configuration for Kubernetes add-ons
│   │   └── vpc                       # VPC configurations for development
│   │       └── terragrunt.hcl        # Terragrunt configuration for VPC
│   ├── provider.tf                   # Provider configuration for Terraform
│   ├── staging                       # Staging environment configurations
│   │   ├── eks                       # EKS specific configurations for staging
│   │   │   └── terragrunt.hcl        # Terragrunt configuration for EKS
│   │   ├── env.hcl                   # Environment specific variables for staging
│   │   ├── kubernetes-addons         # Kubernetes add-ons for staging
│   │   │   └── terragrunt.hcl        # Terragrunt configuration for Kubernetes add-ons
│   │   └── vpc                       # VPC configurations for staging
│   │       └── terragrunt.hcl        # Terragrunt configuration for VPC
│   ├── terraform.tfstate             # Terraform state file (example, typically not versioned)
│   └── terragrunt.hcl                # Root Terragrunt configuration
│
├── infrastructure-modules            # Modules for reusable infrastructure components
│   ├── eks                           # EKS module with configurations
│   │   ├── 0-versions.tf             # Terraform version requirement
│   │   ├── 1-eks.tf                  # EKS resource definitions
│   │   ├── 2-nodes-iam.tf            # IAM roles for EKS nodes
│   │   ├── 3-nodes.tf                # EKS node groups configurations
│   │   ├── 4-irsa.tf                 # IAM roles for service accounts (IRSA)
│   │   ├── 5-outputs.tf              # Output variables for the EKS module
│   │   └── 6-variables.tf            # Input variables for the EKS module
│   ├── kubernetes-addons             # Module for Kubernetes add-ons like autoscaler
│   │   ├── 0-versions.tf
│   │   ├── 1-cluster-autoscaler.tf   # Cluster Autoscaler configuration
│   │   └── 2-variables.tf            # Variables for Kubernetes add-ons module
│   └── vpc                           # VPC module with configurations
│       ├── 0-versions.tf
│       ├── 1-vpc.tf                  # VPC resource definitions
│       ├── 2-igw.tf                  # Internet Gateway configurations
│       ├── 3-subnets.tf              # Subnets configurations
│       ├── 4-nat.tf                  # NAT Gateway configurations
│       ├── 5-routes.tf               # Routing tables and associations
│       ├── 6-outputs.tf              # Output variables for the VPC module
│       └── 7-variables.tf            # Input variables for the VPC module
│
├── nginx-deployment                  # Kubernetes deployment configurations for Nginx
│   ├── config                        # Nginx configuration files
│   │   └── nginx.conf                # Nginx configuration file
│   ├── deployment.yaml               # Kubernetes deployment for Nginx
│   ├── ingress.yaml                  # Ingress resource for Nginx
│   └── service.yaml                  # Service resource for Nginx
│
└── README.md                         # This README file
