# terraform-aws-infra

Terraform project to provision a production-ready AWS infrastructure
using reusable modules. Deploys a VPC with public subnet and an EC2
instance — the foundation for any cloud workload.

## Architecture

```
Internet
    |
Internet Gateway
    |
VPC (10.0.0.0/16)
    |
Public Subnet (10.0.1.0/24)
    |
EC2 Instance (t2.micro)
    |
Security Group (SSH :22, HTTP :80)
```

## Resources Created

| Resource           | Description                          |
|--------------------|--------------------------------------|
| aws_vpc            | VPC with DNS support enabled         |
| aws_subnet         | Public subnet with auto-assign IP    |
| aws_internet_gateway | Allows internet access             |
| aws_route_table    | Routes 0.0.0.0/0 to IGW             |
| aws_security_group | Allows SSH (22) and HTTP (80)        |
| aws_instance       | Amazon Linux 2 EC2, gp3 encrypted    |

## Prerequisites

- Terraform >= 1.3.0
- AWS CLI configured (`aws configure`)
- An existing EC2 key pair in ap-south-1
- An S3 bucket for remote state (or comment out the backend block)

## Usage

```bash
# 1. Clone the repo
git clone https://github.com/rizwanshaikhcr7/terraform-aws-infra.git
cd terraform-aws-infra

# 2. Update terraform.tfvars with your values
vim terraform.tfvars

# 3. Initialise Terraform
terraform init

# 4. Preview the changes
terraform plan

# 5. Apply the infrastructure
terraform apply

# 6. Destroy when done (avoid AWS charges)
terraform destroy
```

## Remote State

State is stored in S3 with DynamoDB locking to support team workflows:

- S3 bucket: `your-terraform-state-bucket`
- DynamoDB table: `terraform-lock`
- State key: `dev/terraform.tfstate`

Update the backend block in `main.tf` with your actual bucket name.

## Variables

| Variable             | Default              | Description                    |
|----------------------|----------------------|--------------------------------|
| aws_region           | ap-south-1           | AWS region                     |
| project_name         | devops-demo          | Used for resource naming/tags  |
| environment          | dev                  | dev / staging / prod           |
| vpc_cidr             | 10.0.0.0/16          | VPC CIDR block                 |
| public_subnet_cidr   | 10.0.1.0/24          | Public subnet CIDR             |
| instance_type        | t2.micro             | EC2 instance type              |
| ami_id               | ami-0f58b397bc5c1f2e8| Amazon Linux 2 (ap-south-1)    |
| key_name             | my-key-pair          | Existing EC2 key pair name     |

## Author

Rizwan Shaikh — [github.com/rizwanshaikhcr7](https://github.com/rizwanshaikhcr7)
