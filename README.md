# Terraform-Cloud-Deploying-A-2-Tier-Architecture-With-CI-CD
"Terraform Cloud"

![image_alt](https://github.com/Tatenda-Prince/Terraform-Cloud-Deploying-A-2-Tier-Architecture-With-CI-CD/blob/9e02bf5dc5d7d087af7564c1d1e8552f5a152ac8/images/Screenshot%202025-01-17%20130535.png)

# Introduction

Deploying a two-tier architecture in AWS can be critical component of any business’s cloud infrastructure. It enables businesses to separate their application layer from their database layer, providing greater security and scalability. While this architecture may seem complex, infrastructure automation tools like Terraform can simplify the process of its creation, configuration and deployment.

In this project, we’ll explore how to use Terraform Cloud to deploy a highly available two-tier AWS architecture. We’ll begin by creating a custom VPC with public and private subnets, launching EC2 instances and setting up an RDS MySQL instance. Furthermore, we’ll demonstrate how custom modules can be used to ensure repeatability and consistency in your infrastructure deployment.

Additionally, we’ll configure security groups and deploy the infrastructure using Terraform Cloud, a powerful CI/CD tool integrated with GitHub to automate the build and deployment process, providing greater efficiency and reliability.

By the end of this project, you’ll be able to create and deploy a scalable and reliable cloud infrastructure on Terraform Cloud, that can handle high traffic loads and maintain uptime, even in the event of failures in individual components.

# Background

## Terraform

In my previous project "Terraform Auto Scaling , I give a good explanation of Terraform, its benefits and some of Terraform’s most fundamental concepts.

I strongly recommend that you read the Introduction, Background and Main Terraform Commands sections of the article before today’s demonstration. This will help you quickly familiarize yourself with the necessary concepts and commands on Terraform so you can be well-prepared to make the most of the demonstration.

## Terraform Modules

Terraform modules are reusable components of infrastructure resources that promote code reusability and consistency. They allow users to define and provision infrastructure resources with ease, making it easier to reuse and share infrastructure code across multiple projects and teams.

## Parent Module

A parent module is a module that uses one or more child modules to define and provision infrastructure resources. It serves as the main entry point and orchestrator for the child modules, providing a way to combine and configure them to create a complete infrastructure environment.

## Child Module

A child module is a reusable component that defines and provisions a specific infrastructure resource, such as a virtual private network or database instance.

Child modules can be called and combined by a parent module to create a complete infrastructure environment, providing a modular and scalable approach to infrastructure management.

## Terraform Cloud

Terraform Cloud is a service that helps developers manage their infrastructure by providing a place to store, organize and collaborate on Terraform files.

It also allows users to manage infrastructure remotely, making it easier to create, update and manage infrastructure resources.

## GitHub

GitHub is a cloud-based service that helps developers to manage and store their code. It empowers users to collaborate with others, manage different versions of their code and track changes over time.

# Prerequisites

1.Fundamental knowledge and understanding of Terraform concepts, modules and commands

2.AWS Account with an IAM user with administrative privileges

3.GitHub account

4.Terraform Cloud account

5.Visual Code Studio 

6.Amazon EC2 Key pair

7.AWS IAM generated Access/Secret Key

## Use Case

You are a Cloud Infrastructure Automation Engineer at Up The Chels Corp, a rapidly growing startup that needs to quickly deploy a scalable and reliable cloud infrastructure to support their new e-commerce platform. The platform is expected to handle high traffic loads and must remain available and responsive to customers at all times.

To achieve this, your manager has tasked you to deploy a two-tier architecture on AWS. You also decided to utilize Terraform Cloud and GitHub leverage its CI/CD tool to automate the build and deployment process, providing greater efficiency and reliability.

# Objectives

## Part 1 (Modules will be utilized to build code for repeatability)

1.Create a custom VPC with 2 Public Subnets for the Web Server Tier, 2 Private Subnets for the RDS Tier

2.Utilize a NAT Gateway in the public subnet and an Internet Gateway for outbound internet traffic

3.Configure a public route table and a private route table

4.Create Security Groups properly configured for needed resources (Application Load Balancer, web servers, RDS instance)

5.Launch an Internet-facing Application Load Balancer targeting the web servers

6.Launch an EC2 Auto Scaling Group of Apache web servers

7.Deploy a RDS MySQL Instance (micro) in a private RDS subnet

## Part 2

1.Deploy using Terraform Cloud as a CI/CD tool and integrate with GitHub account


## Files Structure

Below is the file structure of our Two-Tier Architecture utilizing Terraform modules to ensure repeatability and consistency in your infrastructure deployment.

![image_alt](https://github.com/Tatenda-Prince/Terraform-Cloud-Deploying-A-2-Tier-Architecture-With-CI-CD/blob/823f4eef72150477825990410aa7152d703cdc97/images/Screenshot%202025-01-17%20133401.png)

## Explaining the Terraform Module file structure

The configuration includes a parent module directory with configuration files, along with subdirectories for each child module that include database, ec2_auto_scaling, load_balancer and network_flow. Each module consists of Terraform configuration files that define the specific resources, variables and output values for that module.

We have divided our resources into distinct modules such as VPC, route tables, gateways and security groups in one module, the application load balancer in another, the EC2 Auto Scaling Group in a separate module and the RDS instance in another module. This approach will enable us to efficiently replicate similar infrastructure in the future.

## How we created Custom Modules

For each child module, we define its resources in a separate directory within the parent module directory. In each child module we specify the input variables and outputs in the variable.tf and outputs.tf config files. Afterwards, we reference the child modules in the parent module’s main.tf configuration file and pass in the required argument values in the module block.

The root directory or parent module of the configuration includes essential files such as main.tf, providers.tf, variables.tf and outputs.tf. These files will define the module resources, providers, variables and output values respectively.

## Part 1

## Step 0: Install latest Terraform version in your local machine and log in to Terraform cloud from CLI

## Terraform Cloud Account

If you don’t have an account, sign up for Terraform Cloud here -> https://app.terraform.io/public/signup/account

As we proceed into this article, for a better understanding and to effectively follow this demonstration, kindly fork the project’s repository from my GitHub to yours by clicking on the link below. You are welcome to make any necessary edits to the files from your local visual code studio  CLI, as we progress.

https://github.com/Tatenda-Prince/Terraform-2-Tier-Architecture

## Step 1: Review network configurations child module

In this first step, we are going to review the network_flow module that will include all the networking configurations for our infrastructure. Specifically, we separate the major AWS services and resources to be created into different configurations files, as seen below.

![image_alt](https://github.com/Tatenda-Prince/Terraform-Cloud-Deploying-A-2-Tier-Architecture-With-CI-CD/blob/827e3a247e8f5815bfe760846bee8a445eda89a4/images/Screenshot%202025-01-17%20134407.png)


The vpc.tf file creates a custom VPC that defines our network topology and isolates two public subnets for the web server tier and two private subnets for the RDS tier, each with their own CIDR blocks.

We also create a NAT Gateway in the public subnet for secure access to resources in the private subnet and an Internet Gateway for outbound internet traffic in the gateway.tf config file.

In the route-table.tf config file, we configure public and private route table and associate each subnet with the appropriate route table to determine how traffic is routed in our VPC.

The security-groups.tf config file creates and configures appropriate Security Groups which act as virtual firewalls that control inbound and outbound traffic too and from the resources. Here we have configured a Security Group for our Application Load Balancer, the Auto Scaling Group of web servers and another for our RDS instance, which allows incoming traffic from the appropriate sources.

The file variable.tf declares the variables that will be utilized throughout the configuration. Similarly, the outputs.tf file defines the output values that will be referenced from the parent module and can be passed into other child modules.

## Step 2: Review Application Load Balancer child module

To make our infrastructure more fault-tolerant and scalable, we create an Application Load Balancer (ALB) with the relevant resources in the main.tf which will target our web servers.

![image_alt](https://github.com/Tatenda-Prince/Terraform-Cloud-Deploying-A-2-Tier-Architecture-With-CI-CD/blob/c875afd2ab3336f1e5dee0fe38fec5c46a07ed0d/images/Screenshot%202025-01-17%20134850.png)


## Step 3: Review EC2 Auto Scaling Group child module

This module launches an EC2 Auto Scaling Group in the public subnets and installs an Apache web server using the EC2 user data from a bash script located in the parent module. The instances launched through this module will be publicly accessible and will serve as the platform to host our web content.

![image_alt](https://github.com/Tatenda-Prince/Terraform-Cloud-Deploying-A-2-Tier-Architecture-With-CI-CD/blob/8efd91185ebb6263033c0c76b6b8734509b10248/images/Screenshot%202025-01-17%20134911.png)

## Step 4: Review Database child module

In the module database, we launch an RDS MySQL instance in the private subnets. This instance will be accessible only from within the VPC and incoming traffic will only be allowed from the web servers security group.

![image_alt]()




