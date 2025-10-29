# Terraform

## Core Workflow Commands

### Initialization & Setup

```hcl
terraform init                    # Initialize working directory
terraform init -upgrade           # Upgrade provider plugins
terraform init -reconfigure       # Reconfigure backend

```

### Planning & Validation

```bash
terraform plan                    # Preview changes
terraform plan -out=plan.tfplan   # Save plan to file
terraform plan -destroy           # Preview destroy operation
terraform validate                # Validate configuration syntax
terraform fmt                     # Format code to canonical style
terraform fmt -recursive          # Format all files in directory tree

```

### Apply & Destroy

```bash
terraform apply                   # Apply changes
terraform apply plan.tfplan       # Apply saved plan
terraform apply -auto-approve     # Skip interactive approval
terraform destroy                 # Destroy all resources
terraform destroy -auto-approve   # Destroy without confirmation
terraform apply -target=resource  # Apply to specific resource

```

### State Management

```bash
terraform state list              # List resources in state
terraform state show <resource>   # Show resource details
terraform state mv <src> <dest>   # Move/rename resource in state
terraform state rm <resource>     # Remove resource from state
terraform state pull              # Download and display state
terraform state push              # Upload local state to remote
terraform refresh                 # Update state with real infrastructure

```

### Workspace Management

```bash
terraform workspace list          # List workspaces
terraform workspace new <name>    # Create new workspace
terraform workspace select <name> # Switch workspace
terraform workspace delete <name> # Delete workspace
terraform workspace show          # Show current workspace

```

### Output & Console

```bash
terraform output                  # Show all outputs
terraform output <name>           # Show specific output
terraform console                 # Interactive console for expressions

```

### Import & Taint

```bash
terraform import <resource> <id>  # Import existing infrastructure
terraform taint <resource>        # Mark resource for recreation (deprecated)
terraform untaint <resource>      # Remove taint (deprecated)
terraform apply -replace=resource # Force resource replacement

```

## Configuration Syntax

### Basic Resource Block

```hcl
resource "resource_type" "resource_name" {
  argument1 = "value1"
  argument2 = "value2"

  nested_block {
    nested_arg = "value"
  }
}

```

### Variables

```hcl
# Variable declaration
variable "instance_type" {
  description = "EC2 instance type"
  type        = string
  default     = "t2.micro"
  sensitive   = false
  validation {
    condition     = length(var.instance_type) > 0
    error_message = "Instance type cannot be empty"
  }
}

# Variable usage
instance_type = var.instance_type

```

### Variable Types

```hcl
type = string
type = number
type = bool
type = list(string)
type = map(string)
type = set(string)
type = object({ name = string, age = number })
type = tuple([string, number, bool])
type = any

```

### Outputs

```hcl
output "instance_ip" {
  description = "Public IP of instance"
  value       = aws_instance.example.public_ip
  sensitive   = false
}

```

### Local Values

```hcl
locals {
  common_tags = {
    Environment = "production"
    ManagedBy   = "terraform"
  }
  instance_name = "${var.project_name}-instance"
}

# Usage
tags = local.common_tags

```

### Data Sources

```hcl
data "aws_ami" "ubuntu" {
  most_recent = true
  owners      = ["099720109477"]

  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-*"]
  }
}

# Usage
ami = data.aws_ami.ubuntu.id

```

### Modules

```hcl
# Module declaration
module "vpc" {
  source  = "./modules/vpc"
  version = "1.0.0"

  cidr_block = "10.0.0.0/16"
  name       = "my-vpc"
}

# Module output usage
vpc_id = module.vpc.vpc_id

```

### Provider Configuration

```hcl
terraform {
  required_version = ">= 1.0"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }

  backend "s3" {
    bucket = "my-terraform-state"
    key    = "prod/terraform.tfstate"
    region = "us-east-1"
  }
}

provider "aws" {
  region = "us-east-1"

  default_tags {
    tags = {
      ManagedBy = "Terraform"
    }
  }
}

```

## Expressions & Functions

### String Functions

```hcl
upper("hello")                    # "HELLO"
lower("HELLO")                    # "hello"
title("hello world")              # "Hello World"
trim("  hello  ")                 # "hello"
format("Hello, %s", "World")      # "Hello, World"
join(",", ["a", "b", "c"])        # "a,b,c"
split(",", "a,b,c")               # ["a", "b", "c"]
replace("hello", "l", "L")        # "heLLo"

```

### Collection Functions

```hcl
length([1, 2, 3])                 # 3
concat([1, 2], [3, 4])            # [1, 2, 3, 4]
merge({a=1}, {b=2})               # {a=1, b=2}
lookup({a=1, b=2}, "a", 0)        # 1
keys({a=1, b=2})                  # ["a", "b"]
values({a=1, b=2})                # [1, 2]
contains([1, 2, 3], 2)            # true
distinct([1, 2, 2, 3])            # [1, 2, 3]

```

### Conditional Expressions

```hcl
condition ? true_val : false_val
var.env == "prod" ? "large" : "small"

```

### For Expressions

```hcl
# List transformation
[for item in var.list : upper(item)]

# Map transformation
{for k, v in var.map : k => upper(v)}

# Filtering
[for s in var.list : s if length(s) > 3]

```

### Dynamic Blocks

```hcl
resource "aws_security_group" "example" {
  dynamic "ingress" {
    for_each = var.ingress_rules
    content {
      from_port   = ingress.value.port
      to_port     = ingress.value.port
      protocol    = "tcp"
      cidr_blocks = ingress.value.cidr_blocks
    }
  }
}

```

### Splat Expressions

```hcl
aws_instance.example[*].id        # All instance IDs
aws_instance.example[*].public_ip # All public IPs

```

## Meta-Arguments

### depends_on

```hcl
resource "aws_instance" "example" {
  depends_on = [aws_security_group.example]
}

```

### count

```hcl
resource "aws_instance" "server" {
  count         = 3
  instance_type = "t2.micro"

  tags = {
    Name = "server-${count.index}"
  }
}

```

### for_each

```hcl
resource "aws_instance" "server" {
  for_each = toset(["web", "app", "db"])

  tags = {
    Name = each.key
  }
}

```

### lifecycle

```hcl
resource "aws_instance" "example" {
  lifecycle {
    create_before_destroy = true
    prevent_destroy       = true
    ignore_changes        = [tags]
    replace_triggered_by  = [aws_security_group.example]
  }
}

```

### provisioners

```hcl
resource "aws_instance" "example" {
  provisioner "local-exec" {
    command = "echo ${self.private_ip} >> private_ips.txt"
  }

  provisioner "remote-exec" {
    inline = [
      "sudo apt-get update",
      "sudo apt-get install -y nginx"
    ]
  }

  provisioner "file" {
    source      = "script.sh"
    destination = "/tmp/script.sh"
  }
}

```

## Common Patterns

### Terraform Variables File

```hcl
# terraform.tfvars
region        = "us-east-1"
instance_type = "t2.micro"
environment   = "production"

```

### Environment-Specific Variables

```bash
terraform apply -var-file="prod.tfvars"
terraform apply -var="instance_type=t2.large"

```

### Backend Configuration

```bash
terraform init \
  -backend-config="bucket=my-bucket" \
  -backend-config="key=path/to/state"

```

## Debugging & Logging

```bash
export TF_LOG=TRACE               # Enable detailed logging
export TF_LOG=DEBUG|INFO|WARN|ERROR
export TF_LOG_PATH=./terraform.log # Log to file
terraform apply -parallelism=1    # Reduce parallel operations

```

## Best Practices

- Always run `terraform plan` before `apply`
- Use version constraints for providers and modules
- Store state remotely (S3, Terraform Cloud, etc.)
- Use workspaces or separate state files for environments
- Enable state locking to prevent concurrent modifications
- Use `.terraform.lock.hcl` for dependency locking
- Never commit `.tfstate` files or sensitive `.tfvars` to version control
- Use `terraform fmt` before committing code
- Tag resources consistently for organization and cost tracking