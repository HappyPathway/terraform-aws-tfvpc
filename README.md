# AWS VPC Terraform Module

A Terraform module for creating production-ready VPC configurations in AWS with support for multiple availability zones, subnetting, and network security.

## Features

- Multi-AZ VPC configuration
- Public and private subnet support
- NAT Gateway setup
- VPC Flow Logs
- Network ACLs
- Security group management
- VPC Endpoints support
- IPv6 support (optional)

## Usage

```hcl
module "vpc" {
  source = "HappyPathway/tfvpc/aws"

  vpc_name           = "my-vpc"
  vpc_cidr          = "10.0.0.0/16"
  availability_zones = ["us-west-2a", "us-west-2b"]
  
  # Subnet configuration
  public_subnets  = ["10.0.1.0/24", "10.0.2.0/24"]
  private_subnets = ["10.0.10.0/24", "10.0.11.0/24"]
  
  enable_nat_gateway = true
  enable_vpn_gateway = false
  
  tags = {
    Environment = "production"
    Project     = "example"
  }
}
```

## Requirements

| Name | Version |
|------|---------|
| terraform | >= 1.0.0 |
| aws | >= 4.0.0 |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| vpc_name | Name of the VPC | `string` | n/a | yes |
| vpc_cidr | CIDR block for VPC | `string` | n/a | yes |
| availability_zones | List of AZs | `list(string)` | n/a | yes |
| public_subnets | List of public subnet CIDR blocks | `list(string)` | `[]` | no |
| private_subnets | List of private subnet CIDR blocks | `list(string)` | `[]` | no |
| enable_nat_gateway | Enable NAT Gateway | `bool` | `false` | no |
| enable_vpn_gateway | Enable VPN Gateway | `bool` | `false` | no |
| tags | Tags to apply to resources | `map(string)` | `{}` | no |

## Outputs

| Name | Description |
|------|-------------|
| vpc_id | The ID of the VPC |
| vpc_arn | The ARN of the VPC |
| public_subnet_ids | List of public subnet IDs |
| private_subnet_ids | List of private subnet IDs |
| nat_gateway_ids | List of NAT Gateway IDs |
| vpc_cidr_block | The CIDR block of the VPC |

## Examples

### Basic VPC

```hcl
module "basic_vpc" {
  source = "HappyPathway/tfvpc/aws"

  vpc_name           = "basic-vpc"
  vpc_cidr          = "10.0.0.0/16"
  availability_zones = ["us-west-2a"]
  public_subnets    = ["10.0.1.0/24"]
}
```

### Production VPC

```hcl
module "prod_vpc" {
  source = "HappyPathway/tfvpc/aws"

  vpc_name           = "prod-vpc"
  vpc_cidr          = "10.0.0.0/16"
  availability_zones = ["us-west-2a", "us-west-2b", "us-west-2c"]
  
  public_subnets    = ["10.0.1.0/24", "10.0.2.0/24", "10.0.3.0/24"]
  private_subnets   = ["10.0.10.0/24", "10.0.11.0/24", "10.0.12.0/24"]
  
  enable_nat_gateway = true
  
  tags = {
    Environment = "production"
    Terraform   = "true"
  }
}
```

## Testing

```bash
# Initialize the module
terraform init

# Run tests
terraform test
```

## Contributing

Contributions are welcome! Please read our [Contributing Guide](CONTRIBUTING.md) for details.

## License

MIT License - see [LICENSE](LICENSE) for details