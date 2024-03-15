We''l switch using S3 Bucket and Dynamo DB table to lock our state. 
This approach shuld be used most of the time especially when working in a team, 
instead of relying on the local state.


Keeping scale in mind
Keep data in mind
DevOps Team Work

Stress:
- Networking
- VPCs structure
- k8s
- Security

Followiing the Documentaion:
- AWS
- Terraform
- Terragrunt
- K8s

IaC. Important. Large module should be conssidered harmful.
It's not a good idea to define all in one module.
Largfe modules are slow, insecure, hard to update, difficult to test.


Terragraound Abstraction.

DRY and maintainable Terraform code.
Terragrunt is a thin wrapper that provides extra tools for keeping your configurations DRY, working with multiple Terraform modules, and managing remote state.

Terragrunt helped me to define backend configurations just once.

Safes a lot of time. Keep maintain DRY confurations with large ammout of invironments.


EKS

infrastructure-modules/eks/1-eks.tf
vpc_config {
    endpoint_private_access = false
    endpoint_public_access  = true

    subnet_ids = var.subnet_ids
  }

  I can coonect from my labtop do deploy apps:
  endpoint_public_access  = true

--

Helm Chart Installed

k logs -f  autoscaler-aws-cluster-autoscaler-69847d7574-5jckb  -n kube-system
All is good

--

S3 Bucket created
Allows to go back if something happens.
solitics-devops-terraform-state
arn:aws:s3:::solitics-devops-terraform-state

--

DynamoDB
terraform-lock-table

It allows to avoid conflilcts
While several team members try to rum Terraform simultainisouly.

--

IAM Role Created
Admin Access
arn:aws:iam::864492617736:role/terraform

--

AllowTerraform Policy is created 

{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "Statement",
			"Effect": "Allow",
			"Action": "sts:AssumeRole",
			"Resource": "arn:aws:iam::864492617736:role/terraform"
		}
	]
}

New User creted
Added to DevOps group with all the permissions

solitics-user

Security Credentials generated for the user 
I'll use those credentials to create AWS local profile


## Rub

terragrunt init
terragrint run-all plan 
terragrunt run-all apply