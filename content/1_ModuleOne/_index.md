---
title: "Introduction and Setup"
chapter: true
weight: 2 
---
# Introduction


## Scenario for this workshop
Your company has just embarked on a transition to use more cloud-based infrastructure and database services to increase your company’s infrastructure flexibility while reducing system management costs.
You are one of the key technical leads in the virtual team that’s implementing this transition. You and your team have identified a significant cost savings opportunity by switching to use AWS RDS or Aurora Postgres from costlier legacy database vendors for your modernized applications. Despite the obvious cost savings, however, your company’s Infosec/Risk/Compliance would not sign off on moving the data into AWS Postgres unless an equivalent data privacy and security posture can be achieved on the PostgreSQL as the legacy databases.


## Learning Objectives <!-- MODIFY THIS SUBHEADING -->

In this workshop we will use Baffle to implement some of the most common data security controls required for data privacy and security compliance for PostgreSQL databases on AWS. Because none of these security controls are provided by PostgreSQL natively, application migration and modernization efforts to use PostgreSQL on AWS are often stalled without these security assurances.

## Workshop Structure <!-- MODIFY THIS SUBHEADING -->

this workshop includes 3 related but independent labs that demonstrates Baffle's data protection capabilities for PostgreSQL

* Introduction and setup *(25 minutes)* </li>
* Lab 1: Masking for Dev/Test Environments *(45 minutes)* </li>
* Lab 2: No-code Application Encryption and Dynamic Masking *(45 minutes)* </li>
* Lab 3: Multi-tenant Data Isolation *(45 minutes)* </li>
* Cleanup (5 minutes)
* Conclusion




{{% notice warning %}}
The examples and sample code provided in this workshop are intended to be consumed as instructional content. These will help you understand how various AWS services can be architected to build a solution while demonstrating best practices along the way. These examples are not intended for use in production environments.
{{% /notice %}}
