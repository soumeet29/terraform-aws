## This document explains how terraform variables work. 


If you're familiar with cloudformation, terraform variales area a lot like AWS Cloudformation parameters. When starting out with terraform, you can use hard coded values on your config files(.tf files), but eventually, you will want to start using variables because that way, things become sharable, modular and reusable.

Let's say that you want to create a terraform template that will deploy EC2 instances, and you don't know the exact number of instances you need to deploy.  In these cases you can define default variables within a config file and modify that file to change the number of instances you deploy. Or you can also supply a file with variable values( .tfvars) while running the terraform apply command. 


Here, we will authour three config files, variables.tf, provider.tf and instances.tf. Within provider.tf and instance.tf, we use expression that begins with Var.`<Name>.`    Let's look at come concrete examples 
  
  ### variables.tf
```HCL

variable "region" {
  default = "us-east-2"
}

variable "instance_count" {
	default = 2
}
```

We thus specified that we want to launch our instances in us-east-2 by default and that we want to launch two instances. 

  ### instances.tf
```HCL

resource "aws_instance" "example" {
  ami           = "ami-0c64dd618a49aeee8"
  instance_type = "t2.micro"
  count = var.instance_count

tags = {
    Name = "Terraform_demo"
  }
}


```
In the instance.tf file, we've speciifed the ami name, instance type.  However, instead of hard coding the instance count we are using the value from the variable declared in the variable.tf file. 

#### Note: we've tagged all instances as Terraform_demo, we can iteratively name them using loops. 

  ### provider.tf
```HCL

"aws" {
  # access_key = "ACCESS_KEY_HERE"
  # secret_key = "SECRET_KEY_HERE"
  region     = var.region
}

```
In my environment, I've setup AWS CLI, and my terraform builds can access my AWS account via those credentials. Like above, the region comes from the region variable from the variable.tf file. 

  
  
For more Reference:https://www.terraform.io/docs/configuration/variables.html
