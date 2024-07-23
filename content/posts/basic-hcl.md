---
title: "Terraform: Basic HCL"
date: 2024-07-02
author: "Ryan Rizky Diantoro"
description: "Basic HCL Terraform"
draft: false
categories: [IaC]
tags: [Terraform]
---

## Basic HCL

HashiCorp Configuration Language (HCL) is a domain-specific language used for defining infrastructure as code, primarily in tools like Terraform. Here are the basics of HCL syntax:

### Comments

You can use either # or // for single-line comments, and /* ... */ for multi-line comments.
```hcl
# This is a single-line comment
// This is also a single-line comment

/*
This is a
multi-line comment
*/

```

### Blocks

Blocks are the fundamental building structures in HCL. They start with a keyword followed by a name and enclosed in {}.

```hcl
resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}
```

### Attributes

Attributes are key-value pairs defined within blocks. They can be strings, numbers, lists, or maps.

```hcl
variable "instance_count" {
  description = "Number of instances"
  type        = number
  default     = 1
}

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  count         = var.instance_count
}
```

### Strings

Strings can be defined using double quotes. Interpolation is done with `${}`.

```hcl
resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  tags = {
    Name = "example-${var.environment}"
  }
}
```

### Numbers

Numbers can be integers or floats.

```hcl
resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  count         = 2
}
```

### Lists

Lists are enclosed in square brackets `[]` and elements are separated by commas.

```hcl
variable "availability_zones" {
  type    = list(string)
  default = ["us-west-1a", "us-west-1b"]
}
```

### Maps

Maps are enclosed in curly braces `{}` and key-value pairs are separated by commas.

```hcl
variable "tags" {
  type = map(string)
  default = {
    "Environment" = "dev"
    "Team"        = "devops"
  }
}
```

### Conditionals

Conditionals in HCL are written using the ternary operator.

```hcl
resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  count         = var.enable_instance ? 1 : 0
}
```

### Functions

HCL support a variety of built-in functions.

```hcl
output "instance_ids" {
  value = join(", ", aws_instance.example.*.id)
}
```

### Locals

Locals are used to define reusable expressions.

```hcl
locals {
  instance_name = "example-${var.environment}"
}

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  tags = {
    Name = local.instance_name
  }
}
```