---
title: "Introduction to HCL for Terraform."
seoTitle: "Introduction to HCL for Terraform."
seoDescription: "HCL language code, HCL language code, Terrform, Terraform Blog, Terraform Begginer, DevOPs"
datePublished: Sat Apr 06 2024 16:44:21 GMT+0000 (Coordinated Universal Time)
cuid: cluobrgpo000308ky65p8dc0u
slug: introduction-to-hcl-for-terraform
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1712421651359/de2e7390-f68f-4322-a4c7-bcb9cc487093.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1712421708690/1ff8f474-20e0-41d3-84e6-4a9da779ee3d.png
tags: beginner, devops, terraform, terraform-cloud, hcl, terraweekchallenge

---

Terraform is an infrastructure as code tool created by HashiCorp. It allows you to define and manage your infrastructure resources in a declarative way. HCL (HashiCorp Configuration Language) is the language used to write configurations for Terraform.

Here's a basic overview of HCL syntax and some common constructs:

### **Syntax:**

1. **Blocks**: HCL configurations are organized into blocks. Blocks are groups of statements enclosed in braces`{}`. Each block has a block type and zero or more labels.
    
    Example:
    
    ```plaintext
    block_type "label" {
        # Statements
    }
    ```
    
    The`block_type` represents the type of configuration block. There are several types of blocks in HCL, each serving a specific purpose in defining infrastructure configurations. Some common block types used in Terraform configurations include:
    
    1. **Provider Blocks**: These blocks specify the configuration for a provider, which is responsible for managing resources in a particular infrastructure platform. For example, `provider "aws" { ... }` specifies the configuration for the AWS provider.
        
    2. **Resource Blocks**: Resource blocks define the resources that Terraform should manage, such as virtual machines, databases, networks, etc. Each resource block corresponds to a resource type provided by the chosen provider. For example, `resource "aws_instance" "example" { ... }` defines an AWS EC2 instance.
        
    3. **Data Blocks**: Data blocks are used to retrieve data from a data source, such as existing resources or information from external systems. For example, `data "aws_ami" "example" { ... }` retrieves information about an Amazon Machine Image (AMI).
        
    4. **Variable Blocks**: Variable blocks define input variables that can be used to parameterize the configuration. These variables can be provided externally or set defaults within the configuration. For example, `variable "region" { ... }` defines a variable named `region`.
        
    5. **Output Blocks**: Output blocks define values that should be exposed after the Terraform configuration is applied. These values can be useful for displaying information or passing it on to other systems. For example,`output "instance_ip" { ... }` defines an output variable named`instance_ip`.
        
2. **Attributes**: Inside blocks, you define attributes that have a key-value pair syntax. The key is the attribute name, followed by an equal sign`=`, and then the value.
    
    Example:
    
    ```plaintext
     block_type "label" {
        attribute_name = "value"
    }
    ```
    
    There are several types of attributes commonly used in HCL configurations:
    
    1. **String Attributes**: These attributes represent simple text values enclosed in double quotes (`"`). They are used for specifying text-based configuration settings.
        
        Example:
        
        ```plaintext
        resource "aws_instance" "example" {
            ami           = "ami-12345678"
            instance_type = "t2.micro"
        }
        ```
        
    2. **Numeric Attributes**: Numeric attributes represent integer or floating-point numeric values. They are used for specifying numerical configuration settings.
        
        Example:
        
        ```plaintext
        resource "aws_instance" "example" {
            count = 3
            disk_size_gb = 100
        }
        ```
        
    3. **Boolean Attributes**: Boolean attributes represent true or false values. They are used for specifying binary configuration settings.
        
        Example:
        
        ```plaintext
        resource "aws_instance" "example" {
            monitoring = true
            enable_public_ip = false
        }
        ```
        
    4. **List Attributes**: List attributes represent a sequence of values. They are enclosed in square brackets (`[ ]`) and separated by commas. Lists are used for specifying multiple values for a single configuration setting.
        
        Example:
        
        ```plaintext
        resource "aws_instance" "example" {
            security_groups = ["web", "db"]
            subnet_ids      = ["subnet-12345678", "subnet-87654321"]
        }
        ```
        
    5. **Map Attributes**: Map attributes represent a collection of key-value pairs. They are enclosed in curly braces (`{ }`) and each key-value pair is separated by an equal sign (`=`). Maps are used for specifying complex configurations with multiple settings.
        
        Example:
        
        ```plaintext
        resource "aws_instance" "example" {
            tags = {
                Name        = "example-instance"
                Environment = "production"
            }
            metadata = {
                key1 = "value1"
                key2 = "value2"
            }
        }
        ```
        
    
    These are some of the common types of attributes used in HCL configurations. Depending on the specific Terraform provider and resource being configured, there may be additional attribute types or custom types specific to that resource.
    
3. **Comments**: Comments start with `#` and continue to the end of the line.
    
    Example:
    
    ```plaintext
     # This is a comment
    ```
    

### **Common Constructs:**

1. **Providers**: Providers are plugins that Terraform uses to manage resources. You must declare which provider you're using in your configuration.
    
    Example:
    
    ```plaintext
    provider "aws" {
        region = "us-east-1"
    }
    ```
    
    Different types of providers are available in Terraform, depending on the infrastructure or service you want to manage. Here are some common types of providers:
    
    1. **Cloud Providers**: Cloud providers allow you to manage resources on various cloud platforms like Amazon Web Services (AWS), Microsoft Azure, Google Cloud Platform (GCP), and others.
        
        Example:
        
        ```plaintext
        provider "aws" {
          region = "us-west-2"
        }
        ```
        
    2. **Infrastructure Providers**: Infrastructure providers allow you to manage resources on infrastructure platforms like VMware, OpenStack, or even your local machine.
        
        Example:
        
        ```plaintext
        provider "docker" {
          host = "tcp://localhost:2376/"
        }
        ```
        
    3. **Service Providers**: Service providers allow you to manage services or APIs provided by specific vendors. For example, providers for GitHub, Kubernetes, or Slack.
        
        Example:
        
        ```plaintext
        provider "github" {
          owner  = "organization"
          token  = var.github_token
        }
        ```
        
    4. **Custom Providers**: You can also create custom providers for managing resources or services not covered by existing providers. Custom providers are developed using the Terraform Plugin SDK.
        
        Example:
        
        ```plaintext
        provider "custom" {
          endpoint = "https://example.com/api"
          token    = var.custom_token
        }
        ```
        
    
    Each provider typically requires specific configuration settings, such as authentication credentials, region/zone information, or API endpoints.
    
2. **Resources**: Resources are the basic building blocks of your infrastructure. They represent the cloud components you want to manage.
    
    Example:
    
    ```plaintext
    resource "aws_instance" "example" {
        ami           = "ami-0c55b159cbfafe1f0"
        instance_type = "t2.micro"
    }
    ```
    
    There are various types of resources available, depending on the cloud provider or service being utilized. Here are some common types of resources:
    
    1. **Compute Resources**: These resources represent virtual machines, containers, or compute instances.
        
        Example (AWS EC2 instance):
        
        ```plaintext
        resource "aws_instance" "example" {
          ami           = "ami-12345678"
          instance_type = "t2.micro"
        }
        ```
        
    2. **Storage Resources**: These resources represent storage components such as disks, volumes, or buckets.
        
        Example (Google Cloud Storage Bucket):
        
        ```plaintext
        resource "google_storage_bucket" "example" {
          name     = "example-bucket"
          location = "US"
        }
        ```
        
    3. **Networking Resources**: These resources represent network components such as virtual networks, subnets, or firewalls.
        
        Example (Azure Virtual Network):
        
        ```plaintext
        resource "azurerm_virtual_network" "example" {
          name                = "example-vnet"
          address_space       = ["10.0.0.0/16"]
          location            = "West US"
          resource_group_name = azurerm_resource_group.example.name
        }
        ```
        
    4. **Database Resources**: These resources represent database services such as relational databases, NoSQL databases, or database clusters.
        
        Example (AWS RDS Instance):
        
        ```plaintext
        resource "aws_db_instance" "example" {
          allocated_storage    = 10
          engine               = "mysql"
          instance_class       = "db.t2.micro"
        }
        ```
        
    5. **Security Resources**: These resources represent security-related components such as security groups, access control rules, or encryption keys.
        
        Example (Google Cloud IAM Binding):
        
        ```plaintext
        resource "google_project_iam_binding" "example" {
          project = "my-project-id"
          role    = "roles/editor"
        
          members = [
            "user:example@gmail.com",
            "serviceAccount:123456789012@cloudservices.gserviceaccount.com",
          ]
        }
        ```
        
    
    These are just a few examples of the types of resources you can manage with Terraform. Each cloud provider has its own set of resource types, and Terraform supports a wide range of providers.
    
3. **Variables**: Variables are used to parameterize your configuration, allowing you to reuse values across your Terraform files.
    
    Example:
    
    ```plaintext
    variable "region" {
        description = "The AWS region to deploy to"
        default     = "us-west-2"
    }
    ```
    
    Variables are used to parameterize configurations, allowing you to reuse values across your Terraform files. There are different types of variables available in HCL, each serving a specific purpose. Here are the common types of variables in Terraform:
    
    1. **String Variables**: String variables represent text values. They are used to store textual data such as names, paths, or identifiers.
        
        Example:
        
        ```plaintext
        variable "region" {
          description = "The AWS region to deploy to"
          default     = "us-west-2"
        }
        ```
        
    2. **Numeric Variables**: Numeric variables represent integer or floating-point numeric values. They are used for storing numerical data such as counts, sizes, or capacities.
        
        Example:
        
        ```plaintext
        variable "instance_count" {
          description = "The number of instances to deploy"
          default     = 3
        }
        ```
        
    3. **Boolean Variables**: Boolean variables represent true or false values. They are used for storing binary data, such as enable and disable flags.
        
        Example:
        
        ```plaintext
        variable "enable_monitoring" {
          description = "Enable monitoring"
          default     = true
        }
        ```
        
    4. **List Variables**: List variables represent a sequence of values. They are used for storing multiple values of the same type.
        
        Example:
        
        ```plaintext
        variable "subnets" {
          description = "List of subnets"
          default     = ["subnet-12345678", "subnet-87654321"]
        }
        ```
        
    5. **Map Variables**: Map variables represent a collection of key-value pairs. They are used for storing complex configurations with multiple settings.
        
        Example:
        
        ```plaintext
        variable "tags" {
          description = "Map of resource tags"
          default     = {
            Name        = "example-instance"
            Environment = "production"
          }
        }
        ```
        
    
    These are the common types of variables in HCL. Terraform allows you to define variables to make your configurations more flexible and reusable. You can use variables to customize your infrastructure deployments based on different environments or requirements.
    
4. **Data Sources**: Data sources allow Terraform to fetch information from outside of your configuration, such as existing infrastructure.
    
    Example:
    
    ```plaintext
    data "aws_ami" "example" {
        most_recent = true
    
        filter {
            name   = "name"
            values = ["ubuntu/images/hvm-ssd/ubuntu-trusty-14.04-amd64-server-*"]
        }
    
        filter {
            name   = "virtualization-type"
            values = ["hvm"]
        }
    
        owners = ["099720109477"] # Canonical
    }
    ```
    
    There are various types of data sources available in Terraform, depending on the provider being used. Here are some common types of data sources:
    
    1. **AWS Data Sources**: AWS data sources allow you to fetch information from Amazon Web Services (AWS) such as AMIs, EC2 instances, VPCs, etc.
        
        Example:
        
        ```plaintext
        data "aws_ami" "example" {
          most_recent = true
          owners      = ["self"]
          tags = {
            Name = "example-ami"
          }
        }
        ```
        
    2. **Azure Data Sources**: Azure data sources allow you to fetch information from Microsoft Azure such as virtual machines, virtual networks, etc.
        
        Example:
        
        ```plaintext
        data "azurerm_virtual_network" "example" {
          name                = "example-vnet"
          resource_group_name = "example-resource-group"
        }
        ```
        
    3. **Google Cloud Data Sources**: Google Cloud data sources allow you to fetch information from Google Cloud Platform (GCP), such as GCE instances, GCS buckets, etc.
        
        Example:
        
        ```plaintext
        data "google_compute_instance" "example" {
          name = "example-instance"
        }
        ```
        
    4. **GitHub Data Sources**: GitHub data sources allow you to fetch information from GitHub repositories, such as repository details, pull requests, etc.
        
        Example:
        
        ```plaintext
        data "github_repository" "example" {
          full_name = "owner/repository"
        }
        ```
        
    5. **External Data Sources**: External data sources allow you to call external scripts or programs to fetch data dynamically.
        
        Example:
        
        ```plaintext
        data "external" "example" {
          program = ["python", "${path.module}/script.py"]
        }
        ```
        
    
    These are just a few examples of the types of data sources available in HCL. Each provider typically offers a wide range of data sources to retrieve specific information.
    
5. **Outputs**: Outputs are a way to extract and display information about your infrastructure.
    
    Example:
    
    ```plaintext
    output "instance_ip" {
        value = aws_instance.example.public_ip
    }
    ```
    

There are various types of outputs available in Terraform. Here are the common types:

1. **Value Output**: Value outputs are used to extract specific values from resources or data sources.
    
    Example:
    
    ```plaintext
    output "instance_ip" {
      value = aws_instance.example.public_ip
    }
    ```
    
2. **Attribute Output**: Attribute outputs are used to extract specific attributes from resources or data sources.
    
    Example:
    
    ```plaintext
    output "instance_id" {
      value = aws_instance.example.id
    }
    ```
    
3. **Map Output**: Map outputs are used to group multiple values or attributes into a map.
    
    Example:
    
    ```plaintext
    output "instance_info" {
      value = {
        id         = aws_instance.example.id
        public_ip  = aws_instance.example.public_ip
        private_ip = aws_instance.example.private_ip
      }
    }
    ```
    
4. **List Output**: List outputs are used to group multiple values or attributes into a list.
    
    Example:
    
    ```plaintext
    output "instance_ips" {
      value = [aws_instance.example.*.public_ip]
    }
    ```
    
5. **Computed Output**: Computed outputs are used to calculate or derive values based on expressions or functions.
    
    Example:
    
    ```plaintext
    output "total_cost" {
      value = aws_instance.example.count * aws_instance.example.instance_type.price
    }
    ```
    
6. **Sensitive Output**: Sensitive outputs are used to mark sensitive information that should not be displayed in clear text, such as passwords or access keys.
    
    Example:
    
    ```plaintext
    output "db_password" {
      value     = random_password.example.result
      sensitive = true
    }
    ```
    

These are just a few examples of the types of outputs available in HCL. Outputs allow you to retrieve and expose information about your infrastructure in a structured and controlled manner.

This is just a brief introduction to HCL for Terraform. As you start writing Terraform configurations, you'll encounter more advanced features and best practices. The Terraform documentation is an excellent resource for learning more about HCL and Terraform in general.