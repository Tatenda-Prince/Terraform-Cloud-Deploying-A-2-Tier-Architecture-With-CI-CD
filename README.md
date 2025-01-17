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

In my previous project https://github.com/Tatenda-Prince/Terraform-Deploying-Secure-Highly-Available-Fault-Tolerant-Cloud-Infrastructure, I give a good explanation of Terraform, its benefits and some of Terraform’s most fundamental concepts.

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

![image_alt]()



