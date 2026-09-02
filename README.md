Infrastructure Automation on AWS using Terraform

📌 Project Overview

This project demonstrates how to automate the deployment of AWS infrastructure using Terraform, an Infrastructure as Code (IaC) tool.
The project provisions an Amazon EC2 instance and a Security Group using Terraform configuration files instead of manually creating the resources through the AWS Management Console.

🎯 Objective

The main objective of this project is to understand how Terraform can be used to automate AWS infrastructure deployment.

The project provisions:

One Amazon EC2 instance
One Security Group
SSH access through the Security Group

🛠️ Technologies Used

Terraform
AWS
Amazon EC2
AWS Security Group
AWS CLI
HCL (HashiCorp Configuration Language)

🏗️ Architecture
                 
                  Terraform
                      |
                      v
              AWS Provider
                      |
          +-----------+-----------+
          |                       |
          v                       v
    Security Group            EC2 Instance
      (SSH: 22)             (Ubuntu/Linux)
          |                       |
          +-----------+-----------+
                      |
                      v
                AWS Infrastructure

📁 Project Structure

terraform-aws-ec2/
│
├── main.tf
├── variable.tf
└── README.md

⚙️ Configuration

variable.tf

The variable.tf file contains configurable values used by the Terraform configuration:

variable "region" {
  default = "us-east-1"
}

variable "ami_id" {
  default = "your-ami-id"
}

variable "instance_type" {
  default = "t3.micro"
}

variable "key_name" {
  default = "your-existing-key-name"
}

These variables define the AWS region, AMI, EC2 instance type, and existing EC2 key pair used during deployment.

Note: Replace the AMI ID and key name with values available in your AWS account. The AMI ID is region-specific.

📝 Terraform Configuration

main.tf

The AWS provider is configured using the region variable:

provider "aws" {
  region = var.region
}

A Security Group is created to allow SSH traffic on port 22:

resource "aws_security_group" "allow_ssh" {
  name        = "allow-ssh"
  description = "Allow SSH from anywhere"

  ingress {
    description = "SSH"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

The EC2 instance uses the configured AMI, instance type, key name, and Security Group:

resource "aws_instance" "my_ec2" {
  ami           = var.ami_id
  instance_type = var.instance_type
  key_name      = var.key_name

  vpc_security_group_ids = [
    aws_security_group.allow_ssh.id
  ]

  tags = {
    Name = "Terraform-EC2"
  }
}

The configuration in the project document uses port 22 for SSH access and associates the Security Group with the EC2 instance.

🚀 Deployment Steps

1. Configure AWS CLI

Configure the AWS CLI with the required:

AWS Access Key
AWS Secret Access Key
Region
Output format

The project uses AWS CLI configuration before running Terraform.

2. Create Project Directory

Create a directory for the Terraform project:

mkdir terraform-new
cd terraform-new
3. Create Terraform Files

Create:

main.tf
variable.tf

Add the Terraform configuration to these files.

4. Initialize Terraform

Initialize the Terraform working directory:

terraform init

This prepares Terraform and downloads the required provider dependencies.

5. Validate Configuration

Validate the Terraform configuration:

terraform validate

This checks whether the Terraform configuration is syntactically valid.

6. Create AWS Resources

Apply the Terraform configuration:

terraform apply

Terraform then provisions the configured AWS resources.

7. Verify EC2 Instance

After successful deployment, verify the resource from:

AWS Console
   ↓
EC2
   ↓
Instances

The project verifies that the EC2 instance is running after Terraform deployment.

🔄 Terraform Workflow

Write Terraform Configuration
            ↓
       terraform init
            ↓
      terraform validate
            ↓
       terraform apply
            ↓
    AWS Resources Created
            ↓
      Verify EC2 Instance

✨ Key Features

Infrastructure as Code using Terraform
Automated EC2 deployment
Automated Security Group creation
Configurable infrastructure using Terraform variables
SSH access configuration
AWS provider integration
Reproducible infrastructure deployment

📚 Key Learnings

Through this project, I gained hands-on experience with:

Terraform fundamentals
Infrastructure as Code (IaC)
HCL syntax
AWS provider configuration
EC2 provisioning using Terraform
Security Group configuration
Terraform variables
Terraform initialization and validation
Terraform resource deployment
AWS CLI configuration

🔐 Security Note

The project configuration allows SSH access from:

0.0.0.0/0

This means SSH access is allowed from any IPv4 address.

For a production environment, SSH access should preferably be restricted to a trusted IP address or managed through a more secure access method.

🚀 Future Improvements

Restrict SSH access to a specific IP address.
Add a VPC and subnet configuration.
Use Terraform outputs to display EC2 information.
Use Terraform state management with a remote backend.
Add variables through a terraform.tfvars file.
Add automated infrastructure deployment through a CI/CD pipeline.
Add additional AWS resources such as IAM roles and CloudWatch monitoring.

📌 Project Status

Completed

This project demonstrates how Terraform can be used to automate AWS infrastructure deployment. Using Terraform configuration files, an EC2 instance and Security Group were provisioned without manually creating the resources through the AWS Console.
