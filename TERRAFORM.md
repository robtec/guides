## `providers.tf`
```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}
```

## `main.tf`
```
locals {
  build_path = "build.zip"
  build_hash = filemd5(local.build_path)
  version_name = "${var.application_name}-${random_string.random.result}"
}

provider "aws" {
  region = var.region
}

resource "random_string" "random" {
  length  = 6
  special = false
  lower   = true
  keepers = {
    first = local.build_hash
  }
}
```
## `variables.tf`
```
variable "region" {
  type = string
  default = "eu-west-1"
}
```

## `outputs.tf`
```
```
