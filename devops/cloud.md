- [Terraform](#terraform)
  - [Benefits of Terraform](#benefits-of-terraform)
  - [Terraform Lifecycle](#terraform-lifecycle)
  - [Concept](#concept)
    - [1- Provider](#1--provider)
    - [2- Resources](#2--resources)
    - [3- Module](#3--module)
    - [4- State](#4--state)
    - [5- Variables](#5--variables)
    - [6- Data source](#6--data-source)
  - [Configuration Files](#configuration-files)
    - [1- Configuration file (*.tf files)](#1--configuration-file-tf-files)
    - [2- Variable declaration file (**variables.tf** or **variables.tf.json**)](#2--variable-declaration-file-variablestf-or-variablestfjson)
    - [3- Variable definition files (**terraform.tfvars**)](#3--variable-definition-files-terraformtfvars)
    - [4- State file (**terraform.tfstate**)](#4--state-file-terraformtfstate)

# Terraform

Terraform is one of the most popular Infrastructure-as-code (IaC) tool, used by DevOps teams to automate infrastructure tasks. It is used to automate the provisioning of your cloud resources. Terraform is an open-source, cloud-agnostic provisioning tool developed by HashiCorp and written in GO language.

## Benefits of Terraform

- Does orchestration, not just configuration management
- Supports multiple providers such as AWS, Azure, Oracle, GCP, and many more
- Immutable infrastructure
- Uses easy to understand language, HCL (HashiCorp configuration language)
- Easily portable to any other provider
- Resource Graph

## Terraform Lifecycle

- **init :** initializes the (local) Terraform Environment
- **plan :** compares the Terraform state with as-is state in the cloud.  
Build and display an execution plan (read only).
- **apply :** executes the plan.
- **destroy :** remove all resources and environment of Terraform.

## Concept


### 1- Provider

Such as AWS, Azure, Google Cloud, Docker etc.

### 2- Resources

It is provider services. For example, AWS provider resources; "vpc, s3, ec2 .. etc."

### 3- Module

Any set of Terraform configuration files in a folder is a module.


### 4- State

It is record for changes or information of terraform. Generally, it is recorded in s3 bucket. 

### 5- Variables

Terraform has input and output variables, it is a key-value pair.  
**input variables** are used as parameters to input values at runtime to customize our deployments.  
**output variables** are return values of a terraform module that can be used by other configurations.

### 6- Data source

Data source performs a read-only operation. It allows data to be fetched or computed from resources/entities that are not defined or managed by Terraform or the current Terraform configuration.

## Configuration Files

![images](./img/terraform-config-files.png)

### 1- Configuration file (*.tf files)

Providers and resources are declared here. 

### 2- Variable declaration file (**variables.tf** or **variables.tf.json**)

Here we declare the input variables 

### 3- Variable definition files (**terraform.tfvars**)

Here we assign values to the input variables


### 4- State file (**terraform.tfstate**)
 a state file is created once after Terraform is run. It stores state about our managed infrastructure.
