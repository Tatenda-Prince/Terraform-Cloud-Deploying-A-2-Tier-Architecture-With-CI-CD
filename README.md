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

![image_alt](https://github.com/Tatenda-Prince/Terraform-Cloud-Deploying-A-2-Tier-Architecture-With-CI-CD/blob/f3f34b18ca77a7efd90c4092750d71fab968417d/images/Screenshot%202025-01-17%20134919.png)

In the main.tf config file, we define our RDS instance by specifying the DB engine, instance class and other properties. Here is also where we associate the instance with the appropriate subnet and Security Group.

## Step 5: In depth review of Parent Module

Let’s take an in-depth look at the config files in the parent module that work together to create a two-tier architecture on AWS using module blocks tha take advanatge of the child modules.

## main.tf config file

In this file, each module has a different function — The network_flow module sets up the VPC, the ec2_auto_scaling module sets up the an EC2 Auto Scaling Group of instances with Apache installed, the load_balancer module sets up the Application Load Balancer and the database module sets up the RDS instance. The argument fields for the child module blocks are defined in their respective variable.tf config files.

```python
#Instance Module
module "auto_scaling_group" {
  source               = "./modules/ec2_auto_scaling"
  pub_sub1_id          = module.network_flow.pub_sub1_id
  pub_sub2_id          = module.network_flow.pub_sub2_id
  lt_asg_ami           = "ami-0aa117785d1c1bfe5"
  lt_asg_instance_type = "t2.micro"
  lt_asg_key           = "demoKeypair"
  script_name          = "install-apache.sh"
  asg_sg_id            = module.network_flow.asg_sg_id
  alb_tg_arn           = module.load_balancer.alb_tg_arn
}

#Load Balancer Module
module "load_balancer" {
  source                = "./modules/load_balancer"
  pub_sub1_id           = module.network_flow.pub_sub1_id
  pub_sub2_id           = module.network_flow.pub_sub2_id
  alb_sg_id             = module.network_flow.alb_sg_id
  tg_port               = 80
  tg_protocol           = "HTTP"
  vpc_id                = module.network_flow.vpc_id
  alb_hc_interval       = 60
  alb_hc_path           = "/"
  alb_hc_port           = 80
  alb_hc_timeout        = 45
  alb_hc_protocol       = "HTTP"
  alb_hc_matcher        = "200,202"
  alb_listener_port     = "80"
  alb_listener_protocol = "HTTP"
}

#VPC Module
module "network_flow" {
  source         = "./modules/network_flow"
  vpc_cidr       = var.pm_vpc_cidr
  pub_sub1_cidr  = "10.0.1.0/24"
  pub_sub2_cidr  = "10.0.2.0/24"
  priv_sub1_cidr = "10.0.3.0/24"
  priv_sub2_cidr = "10.0.4.0/24"
  map_public_ip  = true
  az_1           = "us-east-1a"
  az_2           = "us-east-1b"
  pub_rt_cidr    = "0.0.0.0/0"
  priv_rt_cidr   = "0.0.0.0/0"
}


#Data Module
module "database" {
  source                      = "./modules/database"
  priv_sub1_id                 = module.network_flow.priv_sub1_id
  priv_sub2_id                 = module.network_flow.priv_sub2_id
  allocated_storage           = 5
  storage_type                = "gp2"
  engine                      = "mysql"
  engine_version              = "5.7"
  instance_class              = "db.t2.micro"
  vpc_security_group_ids      = module.network_flow.db_sg_id
  parameter_group_name        = "default.mysql5.7"
  username                    = "two_tier_db"
  db_name                     = var.db_username
  password                    = var.db_password
  allow_major_version_upgrade = true
  auto_minor_version_upgrade  = true
  backup_retention_period     = 35
  backup_window               = "22:00-23:00"
  maintenance_window          = "Sat:00:00-Sat:03:00"
  multi_az                    = "false"
  skip_final_snapshot         = true
}
```

## Code Explanation

## network_flow module

This module sets up the Virtual Private Cloud (VPC) and its associated resources, including two public subnets, two private subnets, internet and NAT gateways and route tables for each subnet. It also creates security groups for the various components, including one for the EC2 instances and one for the RDS instance.

## auto_scaling_group module

This module creates an EC2 Auto Scaling Group in the public subnets of the VPC created by the network_flow module. It installs an Apache web server using a Bash script specified in the script_name variable. The Auto Scaling Group launches instances with the specified Amazon Machine Image (AMI), instance type, key and security group.

## load_balancer module

This module creates an Application Load Balancer in the public subnets of the VPC created by the network_flow module. It sets up a Target group for the EC2 instances created by the auto_scaling_group module and creates a listener on port 80 to route incoming traffic to the Target group.

## database module

This module creates an Amazon RDS instance in the private subnets of the VPC created by the network_flow module. It specifies the database engine (MySQL), instance class, allocated storage, username, password, backup and maintenance windows and other settings.

## variables.tf config file

In this config file we declare variable blocks that define variables used in the configuration.

```python
#####################################
#VPC Variables - Parent Module 
####################################

variable "pm_vpc_cidr" {
  default = "10.0.0.0/16"
  type    = string
}

variable "db_username" {}
variable "db_password" {}
```

## Code Explanation

The first variable pm_vpc_cidr sets the default value for the VPC CIDR block as 10.0.0.0/16. This variable is of type string and if no value is provided for this variable during runtime, it will use the default value.

The db_username and db_password is also declared, however we will set these values when we integrate Terraform Cloud as we can configure them to be secret values.


## output.tf config file

The output block in Terraform is used to define values that can be queried or used by other parts of the configuration.

```python
output "alb-dns" {
  value = module.load_balancer.alb_dns
}
```

## Code Explanation

In this specific case, we define an output called alb-dns and set its value to the DNS name of the Application Load Balancer (ALB) created by the load_balancer module. After successfully applying the Terraform configuration, this value will be outputted and we can use it to access the ALB, which serves content from the web servers, from our browser, .

## Part 2

## Deploy the Infrastructure using Terraform Cloud

We have chosen Terraform Cloud as our CI/CD tool to simplify our infrastructure deployment process. With its centralized location for managing and sharing infrastructure code and built-in support for continuous integration and delivery, we can deploy changes to production more efficiently.

Terraform Cloud integrates with popular source control tools like GitHub, making it easy for our teams to trigger infrastructure changes automatically when code changes are made. Additionally, its features for ensuring the reliability and security of our infrastructure give us peace of mind that our systems are always up-to-date and secure. By leveraging Terraform Cloud, we can streamline our infrastructure code management, deploy changes more quickly and maintain control over our infrastructure

## Step 1: Sign into Terraform Cloud Account and create a workspace

## Sign in to Terraform Cloud

Head to the Terraform Cloud sign in page to sign into your account. Input your sign in credentials, then click Sign in.

![image_alt](https://github.com/Tatenda-Prince/Terraform-Cloud-Deploying-A-2-Tier-Architecture-With-CI-CD/blob/3625fa9a56bd44e1bb30e16221500cf2c231782e/images/Screenshot%202025-01-17%20145310.png)


## Create an new organization

Next, we will create and name a new organization. Add your valid email address, then click Create organization.


![image_alt](https://github.com/Tatenda-Prince/Terraform-Cloud-Deploying-A-2-Tier-Architecture-With-CI-CD/blob/e027b98a782e78ddb5410811e876ef304eda4968/images/Screenshot%202025-01-04%20124931.png)

## Create a workspace

In the Projects and workspaces tab select Create a workspace, then slelect Version control workflow.

![image_alt](https://github.com/Tatenda-Prince/Terraform-Cloud-Deploying-A-2-Tier-Architecture-With-CI-CD/blob/62de571410d82c19a53d02d9e185c948428ad7f5/images/Screenshot%202025-01-04%20125112.png)

## Configure VCS settings to integrate with GitHub

For the Connect to VCS step, select GitHub or any other preferred version control provider. For this demonstration we will use GitHub.

![image_alt](https://github.com/Tatenda-Prince/Terraform-Cloud-Deploying-A-2-Tier-Architecture-With-CI-CD/blob/e98fb53b38c90a246daacc7dfba469588bc2e40a/images/Screenshot%202025-01-04%20125837.png) 

Select your GitHub repository, then click Install to authorize Terraform Cloud.

![image_alt](https://github.com/Tatenda-Prince/Terraform-Cloud-Deploying-A-2-Tier-Architecture-With-CI-CD/blob/57e533648be51aed4da57caf0407212ebe24639e/images/Screenshot%202025-01-04%20130426.png)

Let’s continue by selecting your GitHub Repo, then continuing to Configure settings.

Give your workspace a name, scroll down, select Advanced options, then make sure Manual apply and Apply trigger runs are selected.

![image_alt](https://github.com/Tatenda-Prince/Terraform-Cloud-Deploying-A-2-Tier-Architecture-With-CI-CD/blob/1a1373b8041f63480894a7629c3efe81c6c991a5/images/Screenshot%202025-01-04%20130958.png)


Next, scroll down, then clicking Create workspaces.

![image_alt](https://github.com/Tatenda-Prince/Terraform-Cloud-Deploying-A-2-Tier-Architecture-With-CI-CD/blob/a15b8c1595b33eb49016ed7811fbd1b9f6af6d4d/images/Screenshot%202025-01-17%20141704.png)


## Step 3: Set up our environment variables

To be able to have access to our AWS environment and ensure the security of our AWS access credentials (access/secret key), we will configure the following variables as sensitive — AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY and AWS_DEFAULT_REGION.

Click on Go to workspace overview.

![image_alt](https://github.com/Tatenda-Prince/Terraform-Cloud-Deploying-A-2-Tier-Architecture-With-CI-CD/blob/4b8375d494229ab135021a22c76f0be8508b1290/images/Screenshot%202025-01-17%20141934.png)

Select the Variables tab on the left panel, scroll down then click Add variable, as seen in the example below of using AWS access keys—

Note — Make sure to select Environment variables for the variable category when setting the AWS access keys.

AWS_ACCESS_KEY_ID example —

![image_alt](https://github.com/Tatenda-Prince/Terraform-Cloud-Deploying-A-2-Tier-Architecture-With-CI-CD/blob/afd6065d3fdace35ac7465f2d54a860447b3a971/images/Screenshot%202025-01-17%20142501.png)

Additionally, add the Terraform variables db_username and db_password which would be used to set the Database’s username and password. Remember to set their values to sensitive.

Once completed, you will have defined five variables in Terraform Cloud, as seen below.

![image_alt](https://github.com/Tatenda-Prince/Terraform-Cloud-Deploying-A-2-Tier-Architecture-With-CI-CD/blob/d01e7d275f2fe8560262c91d8711bc9506b813ae/images/Screenshot%202025-01-17%20142511.png)

## Step 4: Execute the Terraform configuration code

Let’s get to action! On the right hand side, click Actions, then Start new run to execute our Terraform code and deploy the infrastructure in our AWS environment.

![image_alt](https://github.com/Tatenda-Prince/Terraform-Cloud-Deploying-A-2-Tier-Architecture-With-CI-CD/blob/ebb7fb4ba101785da402911372ee1983acb1cd5e/images/Screenshot%202025-01-04%20135957.png)

In the next prompt, type your Reason for starting run, then click Start run.

![image_alt](https://github.com/Tatenda-Prince/Terraform-Cloud-Deploying-A-2-Tier-Architecture-With-CI-CD/blob/ef0e8e82fe9c4280d5647ab6393cade929c999c1/images/Screenshot%202025-01-04%20140607.png) 



Upon successful completion of the plan, you will see a green bar accompanied by a green checkmark labeled as Plan Finished. This step is akin to running the terraform plan command, which displays a preview of the alterations that will be made to your AWS infrastructure based on the current Terraform configuration.

Continue by scrolling down and clicking Confirm and Apply. Add a comment, then click Confirm Plan to deploy our AWS infrastructure.

![image_alt](https://github.com/Tatenda-Prince/Terraform-Cloud-Deploying-A-2-Tier-Architecture-With-CI-CD/blob/ced32374211119afb528b78a043b45b8e94cbe79/images/Screenshot%202025-01-04%20140628.png) 


Terraform Cloud should now start to run the Apply.

![image_alt](https://github.com/Tatenda-Prince/Terraform-Cloud-Deploying-A-2-Tier-Architecture-With-CI-CD/blob/28f0995b0612e6d7eea1b102399009f32449cc42/images/Screenshot%202025-01-04%20192752.png)


Apply Finished! The application process is now complete! Terraform Cloud has effectively deployed our two-tier architecture in the AWS environment. You should now be able to access and inspect all of the newly created resources.

![image_alt](https://github.com/Tatenda-Prince/Terraform-Cloud-Deploying-A-2-Tier-Architecture-With-CI-CD/blob/68faeb66e601ec57272a3ffaecf514fea0a22e9c/images/Screenshot%202025-01-04%20193756.png)

Scroll down and take note of the Outputs value of the DNS of the created Application Load Balancer. Copy and save the ALB’s DNS, as it will be required to access the web page from the browser.

![image_alt](https://github.com/Tatenda-Prince/Terraform-Cloud-Deploying-A-2-Tier-Architecture-With-CI-CD/blob/c151246178c408226c801807a13eade4a59ecb97/images/Screenshot%202025-01-04%20193824.png)

Now, let’s verify that our resources have been created by reviewing them in the Management Console.

# Step 5: Verify AWS resources were created in Management Counsel

In the AWS Management Console, head to the EC2 dashboard and verify that the desired two EC2 web servers were launched from the ASG.

Great! Looks like we are in business here!

![image_alt](https://github.com/Tatenda-Prince/Terraform-Cloud-Deploying-A-2-Tier-Architecture-With-CI-CD/blob/3e863558ab3b03cef9815b3ae7458dec0bb27920/images/Screenshot%202025-01-04%20193904.png)

Next, navigate to the left pane, scroll down and select Target groups. Select the newly created Target group, scroll down, then verify that the instances Health status is healthy, as shown below.

Awesome! Our ASG EC2 instances are all healthy.

![image alt](https://github.com/Tatenda-Prince/Terraform-Cloud-Deploying-A-2-Tier-Architecture-With-CI-CD/blob/75c2c82e3830bbea335ab4b47f90ced96f528717/images/Screenshot%202025-01-04%20193959.png)

Lastly, head to the RDS service dashboard and verify that the database has been launched.

Yes! There is our MySQL Database instance, up and running!

![image_alt](https://github.com/Tatenda-Prince/Terraform-Cloud-Deploying-A-2-Tier-Architecture-With-CI-CD/blob/d369c9a324405d4740893d7d4d3786d4e2a274a3/images/Screenshot%202025-01-04%20194048.png)

Great! We’ve successfully confirmed that our ASG has generated the expected EC2 instances and all of our Target groups’ health statuses are displaying as healthy.

Let’s now verify that we can access our ASG’s web servers through our ALB.

# Step 10: Verify reachability to the Web server using its URL in browser

Open up your desired browser and paste the ALB’s DNS in your browser.

Note — Make sure to use the “http://” protocol and not https:// to reach the ALB.

![image_alt](https://github.com/Tatenda-Prince/Terraform-Cloud-Deploying-A-2-Tier-Architecture-With-CI-CD/blob/7fb2eef187dccc0f61c6aac0f3fcfead95124972/images/Screenshot%202025-01-04%20194149.png)

## Congratulations


You’ve successfully completed “The Terraform Cloud!”

Creating a highly available two-tier AWS architecture can be complex, but with Terraform, it becomes much more manageable. By following the steps outlined in this article, we’ve easily created a custom VPC, launch EC2 instances, set up an RDS MySQL instance and deploy your infrastructure using Terraform Cloud.

## Bonus

## Commit changes to code in GitHub and trigger run in Terraform Cloud.

If you wish to take things a step further, you can modify the local repository, save the changes and then push them to the GitHub repository. As a result, a run will be initiated automatically within Terraform Cloud, since it will retrieve the code from GitHub and proceed to deploy the infrastructure to AWS.

## Clean up

## Destroy infrastructure

![image_alt](https://github.com/Tatenda-Prince/Terraform-Cloud-Deploying-A-2-Tier-Architecture-With-CI-CD/blob/64b3fece535ea42f683724ec9e37d2df5de7e2bc/images/Screenshot%202025-01-04%20194309.png)

Select Queue destroy plan, enter your workspace name to confirm, then click Queue destroy plan.

Scroll down, click Confirm and Apply, add a message, then click Confirm Plan to start the process of destroying of all previously provisions resources from Terraform Cloud.









































