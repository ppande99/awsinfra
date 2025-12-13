# AWS Networking Lab

This Terraform configuration creates a simple AWS networking stack for learning purposes:

- VPC with DNS support enabled
- Two public and two private subnets (modifiable via variables)
- Internet gateway for public egress
- NAT gateway for private subnet egress
- Separate public and private route tables with associations

## Prerequisites
- [Terraform](https://developer.hashicorp.com/terraform/downloads) v1.5.0 or newer
- AWS credentials configured via environment variables or an AWS CLI profile

## Usage
1. Initialize the working directory:
   ```bash
   terraform init
   ```

2. Review and customize variables in `variables.tf` or via a `terraform.tfvars` file. Key values:
   - `aws_region` (default: `us-east-1`)
   - `aws_profile` (optional AWS CLI profile name)
   - `vpc_cidr` (default: `10.0.0.0/16`)
   - `public_subnets` and `private_subnets` (CIDR lists; lengths should match)

3. Preview the changes:
   ```bash
   terraform plan
   ```

4. Apply the infrastructure:
   ```bash
   terraform apply
   ```

## Outputs
- `vpc_id`
- `public_subnet_ids`
- `private_subnet_ids`
- `public_route_table_id`
- `private_route_table_id`
