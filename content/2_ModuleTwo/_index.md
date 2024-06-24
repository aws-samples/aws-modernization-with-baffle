---
title: "Lab 1 - Masking for DEV/TEST Environments" # MODIFY THIS TITLE IF APPLICABLE
chapter: true
weight: 4 
---

# Lab 1 - MASKING FOR DEV/TEST ENVIRONMENTS

## Scenario  
One of the first steps of the modernization process is to deploy a test/dev  
environment on AWS for the application team to develop the updated  
version of the application and test those updates in the new cloud  
infrastructure. Since it’s a test/dev environment, the database must not  
contain any sensitive values So, you decide to use Baffle’s static masking  
feature to anonymize sensitive data as you copy the data from production  
databases into the new PostgreSQL database so that you can do both in one  
workflow.  

## Goals for this lab  
After completing this lab, you will have a PostgreSQL database with a  
statically masked dataset created from a cleartext dataset. You will be able  
to verify that the demo application can work directly against masked  
database and see only masked values for sensitive data fields.  

## Brief overview of Lab 1  
![StaticMaskingDiag](../images/StaticMaskingLowereEnv.png)
During this lab, you will perform the following steps to enable a continuous static  
data masking workflow to create an anonymized dataset on a test/dev database:  
- Enable masking policies  
- Trigger DMS job to perform initial masking of data  
- Verify the sample application works with masked data  
- Add new data to production database and verify that new data is  
automatically transferred and masked in test/dev database  

The setup process you initiated at the beginning of the workshop will prepopulate  
your Baffle Manager with the masking policies needed. 
