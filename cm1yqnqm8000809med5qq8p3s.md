---
title: "AWSCloudAction: Automate Your CI/CD Workflows Deploying GitHub Actions Runners on AWS with Terraform"
datePublished: Mon Oct 07 2024 08:17:00 GMT+0000 (Coordinated Universal Time)
cuid: cm1yqnqm8000809med5qq8p3s
slug: awscloudaction-automate-your-cicd-workflows-deploying-github-actions-runners-on-aws-with-terraform
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1728288877664/42dbfd6f-ec55-4885-bd88-07c95fd8d04d.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1728288946714/584540af-666a-49f7-a80b-a7705ddf7846.png
tags: aws, github, devops, terraform, github-actions-1

---

In the fast-paced world of software development, Continuous Integration and Continuous Deployment (CI/CD) have become crucial for delivering high-quality software quickly. GitHub Actions is a powerful tool that enables developers to automate workflows directly within their GitHub repositories. This article will guide you through automating the deployment of GitHub Actions runners on AWS using Terraform, an Infrastructure as Code (IaC) tool.

## Why Deploy Your Own GitHub Actions Runners?

While GitHub offers hosted runners, there are several benefits to deploying your own:

* **Customization**: Tailor the environment to suit your project requirements.
    
* **Cost Efficiency**: Reduce costs, especially for frequent builds.
    
* **Performance Control**: Ensure your runners meet performance standards for your workflows.
    

## Project Architecture

The architecture of our project consists of several key components:

1. **AWS EC2 Instance**: The GitHub Actions runner will be hosted on an EC2 instance.
    
2. **IAM Role**: An IAM role with permissions for the runner to execute jobs and access AWS resources.
    
3. **Security Group**: A security group to control access to the EC2 instance.
    
4. **CloudWatch**: Monitoring and logging capabilities to track the runner’s performance.
    

![Screenshot from 2024-10-07 13-32-26.png](https://github.com/Rudraksh2003/AWSCloudAction/blob/main/Screenshot%20from%202024-10-07%2013-32-26.png?raw=true align="left")

  

## Prerequisites

Before you begin, ensure you have the following:

* An AWS account.
    
* Terraform installed on your local machine.
    
* A GitHub account and a repository to configure the Actions runner.
    
* A GitHub Personal Access Token with the `repo` and `workflow` scopes.
    
* An SSH key pair for accessing the EC2 instance.
    

## Step-by-Step Implementation

### Step 1: Setting Up the Terraform Project

Create a directory for your Terraform project:

```bash
mkdir github-actions-runner-aws
cd github-actions-runner-aws
```

Create the following necessary files:

```plaintext
github-actions-runner-aws/
├── main.tf
├── variables.tf
├── iam.tf
├── ec2.tf
├── security_group.tf
└── cloudwatch.tf
```

### Step 2: Define the Terraform Configuration

#### [`main.tf`](http://main.tf)

This file is the main entry point for your Terraform configuration, tying together all resources.

```plaintext
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.0"
    }
  }
  required_version = ">= 1.0"
}

provider "aws" {
  region = var.region
}

module "iam_role" {
  source = "./iam.tf"
}

module "ec2_instance" {
  source     = "./ec2.tf"
  iam_role   = module.iam_role.iam_role_name
}

module "security_group" {
  source = "./security_group.tf"
}

module "cloudwatch" {
  source      = "./cloudwatch.tf"
  instance_id = module.ec2_instance.instance_id
}
```

* `terraform` block: Specifies the required provider (AWS) and version.
    
* `provider` block: Configures the AWS provider with the specified region.
    
* **Modules**: Includes IAM role, EC2 instance, security group, and CloudWatch configurations.
    

#### [`variables.tf`](http://variables.tf)

This file defines the required variables for your configuration.

```plaintext
variable "region" {
  description = "The AWS region to deploy the resources"
  type        = string
  default     = "us-east-1"
}

variable "instance_type" {
  description = "EC2 instance type"
  type        = string
  default     = "t2.micro"
}

variable "github_token" {
  description = "GitHub personal access token for authentication"
  type        = string
}

variable "runner_name" {
  description = "Name of the GitHub Actions runner"
  type        = string
  default     = "my-runner"
}
```

* `region`: Defines the AWS region for deployment.
    
* `instance_type`: Specifies the EC2 instance type.
    
* `github_token`: Token for GitHub authentication.
    
* `runner_name`: Name of the GitHub Actions runner.
    

#### [`iam.tf`](http://iam.tf)

This file creates an IAM role with the necessary policies for your GitHub Actions runner.

```plaintext
resource "aws_iam_role" "github_runner_role" {
  name = "github-runner-role"

  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = "sts:AssumeRole"
        Principal = {
          Service = "ec2.amazonaws.com"
        }
        Effect = "Allow"
        Sid    = ""
      },
    ]
  })
}

resource "aws_iam_policy" "github_runner_policy" {
  name        = "github-runner-policy"
  description = "Policy for GitHub runner to access CloudWatch logs"
  policy      = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Action = [
          "logs:*",
        ]
        Resource = "*"
      },
    ]
  })
}

resource "aws_iam_role_policy_attachment" "attach_policy" {
  policy_arn = aws_iam_policy.github_runner_policy.arn
  role       = aws_iam_role.github_runner_role.name
}
```

* **IAM Role**: Allows EC2 to assume the role.
    
* **IAM Policy**: Grants access to CloudWatch logs.
    
* **Role Policy Attachment**: Attaches the policy to the role.
    

#### [`ec2.tf`](http://ec2.tf)

This file defines the EC2 instance that will host the GitHub Actions runner.

```plaintext
resource "aws_instance" "github_runner" {
  ami           = "ami-0c55b159cbfafe1f0" # Update this with the latest Amazon Linux 2 AMI
  instance_type = var.instance_type
  key_name      = var.key_name
  subnet_id     = aws_subnet.public_subnet.id

  security_groups = [
    aws_security_group.github_runner_sg.name
  ]

  iam_instance_profile = aws_iam_role.github_runner_role.name

  tags = {
    Name = var.runner_name
  }

  provisioner "remote-exec" {
    inline = [
      "cd /home/ec2-user && mkdir actions-runner && cd actions-runner",
      "curl -o actions-runner-linux-x64-2.283.0.tar.gz -L https://github.com/actions/runner/releases/download/v2.283.0/actions-runner-linux-x64-2.283.0.tar.gz",
      "tar xzf ./actions-runner-linux-x64-2.283.0.tar.gz",
      "./bin/installdependencies.sh",
      "./config.sh --url https://github.com/YOUR_USERNAME/YOUR_REPOSITORY --token ${var.github_token}",
      "sudo ./svc.sh install",
      "sudo ./svc.sh start",
    ]
  }
}
```

* **AMI**: Specify the Amazon Machine Image for the EC2 instance.
    
* **Instance Type**: Set the instance type (e.g., `t2.micro`).
    
* **Security Groups**: Associates the instance with the security group.
    
* **IAM Role**: Attaches the IAM role to the EC2 instance.
    
* **Provisioner**: Runs commands to set up the GitHub Actions runner on the instance.
    

#### `security_`[`group.tf`](http://group.tf)

This file configures a security group for your EC2 instance.

```plaintext
resource "aws_security_group" "github_runner_sg" {
  name        = "github-runner-sg"
  description = "Allow SSH and HTTP traffic"
  vpc_id      = aws_vpc.my_vpc.id

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"] # Replace with your IP range for security
  }

  ingress {
    from_port   = 80
    to_port     = 80
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
```

* **Security Group**: Defines rules for inbound and outbound traffic.
    
* **Ingress Rules**: Allow SSH (port 22) and HTTP (port 80) traffic.
    
* **Egress Rules**: Allow all outbound traffic.
    

#### [`cloudwatch.tf`](http://cloudwatch.tf)

This file configures CloudWatch for monitoring and logging.

```plaintext
resource "aws_cloudwatch_log_group" "github_runner_logs" {
  name              = "/aws/ec2/github-runner"
  retention_in_days = 14
}

resource "aws_cloudwatch_metric_alarm" "high_cpu_alarm" {
  alarm_name          = "HighCPUUsage"
  alarm_description   = "This alarm triggers when CPU usage exceeds 80%"
  namespace           = "AWS/EC2"
  metric_name         = "CPUUtilization"
  dimensions          = {
    InstanceId = aws_instance.github_runner.id
  }
  statistic           = "Average"
  period              = 300
  evaluation

_periods  = 1
  threshold           = 80
  comparison_operator = "GreaterThanThreshold"

  alarm_actions = ["arn:aws:sns:us-east-1:123456789012:MySNSTopic"] # Replace with your SNS topic ARN
}
```

* **CloudWatch Log Group**: Creates a log group for the runner logs.
    
* **CloudWatch Alarm**: Monitors CPU usage and triggers if it exceeds a defined threshold.
    

### Step 3: Initialize and Apply the Terraform Configuration

Run the following commands to deploy the infrastructure:

```bash
# Initialize Terraform
terraform init

# Plan the deployment
terraform plan -var "github_token=YOUR_GITHUB_TOKEN"

# Apply the configuration
terraform apply -var "github_token=YOUR_GITHUB_TOKEN"
```

### Step 4: Configure Your GitHub Repository

After the EC2 instance is deployed, go to your GitHub repository settings:

1. Navigate to **Settings** &gt; **Actions** &gt; **Runners**.
    
2. Click on **Add Runner**.
    
3. Follow the instructions provided in the runner configuration to register your newly created runner.
    

### Monitoring and Logging

Consider implementing monitoring and logging for your runners. Utilize CloudWatch to track metrics and logs. Set up alarms to notify you of performance issues or errors.

## Conclusion

By deploying your own GitHub Actions runners on AWS using Terraform, you gain enhanced control over your CI/CD processes. This setup allows you to optimize costs, customize your build environments, and maintain performance standards. As software development continues to evolve, having the ability to deploy and manage your own CI/CD infrastructure will give you a significant advantage.

By leveraging Terraform for infrastructure management, you ensure that your deployments are reproducible, scalable, and easily maintainable. Whether you are working on small projects or large-scale applications, deploying your own GitHub Actions runners can enhance your development workflow significantly.

---