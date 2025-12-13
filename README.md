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

## GitHub Actions pipeline
A GitHub Actions workflow (`.github/workflows/terraform.yml`) formats, validates, and plans Terraform on pull requests and pushes to `main`. A manual dispatch (with `apply=true`) runs `terraform apply`.

### Configure AWS access with OIDC
1. Create an IAM role that trusts GitHub’s OIDC provider. Limit the trust policy to your repository and (optionally) branch environment.
2. Attach permissions for the networking resources (e.g., VPC, subnets, gateways, route tables, Elastic IPs). Least privilege is recommended.
3. Add a repository secret `AWS_OIDC_ROLE_ARN` containing the role ARN.
4. If you prefer a different region than `us-east-1`, update the `AWS_REGION` env in the workflow or set `aws_region` in `terraform.tfvars`.

### Running the workflow
- Pull requests and pushes to `main` automatically run `terraform fmt -check`, `terraform init`, `terraform validate`, and `terraform plan`, uploading the plan as an artifact.
- To create/update resources from GitHub, trigger the workflow manually and set the `apply` input to `true`. The job will assume the configured IAM role, run `terraform init`, and then `terraform apply -auto-approve`.

## Outputs
- `vpc_id`
- `public_subnet_ids`
- `private_subnet_ids`
- `public_route_table_id`
- `private_route_table_id`
