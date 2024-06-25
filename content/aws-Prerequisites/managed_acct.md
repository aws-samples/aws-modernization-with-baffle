---
title: "Using your own AWS account"
chapter: true
weight: 3 
---
# Using your own AWS account

To run this workshop successfully, we will need to run through a few steps to properly set up and configure the environment. These steps include provisioning the following resources using Cloudformation templates

 1. Create an AWS IAM group with relevant policies.
 2. Provision the following resources 
    * RDS Postgres instance
    * DMS instance
    * EC2 instance with Baffle software
    * AWS Secret Manger
    * AWS KMS
    * AWS S3 


Prerequisites to run Cloudformation are:

If you are using a Managed AWS account please make sure to have privileges to access and/or create above mentioned AWS resources.


{{% notice warning %}}
Provisioning this workshop environment in your AWS account will create resources and **there will be costs associated with them**. The cleanup section provides a guide to remove them, preventing further charges.
{{% /notice %}}
