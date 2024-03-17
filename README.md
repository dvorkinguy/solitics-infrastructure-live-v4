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