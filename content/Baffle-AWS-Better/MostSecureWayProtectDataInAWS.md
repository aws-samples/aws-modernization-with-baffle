---
title: "The Most Secure Way to Protect Data in AWS"
chapter: true
weight: 5
---

## AWS & Baffle: Secure Your Cloud Databases

Challenge: Moving relational databases to the cloud securely, especially with regulations requiring "least privilege" access.

Solution:  AWS + Baffle

-   AWS RDS/Aurora: Provides a managed PostgreSQL foundation in the cloud.
    
-   Baffle: Encrypts sensitive data without code changes, enabling least privilege for all users, DBAs, and cloud admins.
    
-   Centralized Management: Baffle offers centralized key and policy control for simplified administration.
    

Benefit: Secure transition to the cloud with strong encryption and granular access control.


-   99% of applications require advanced SQL queries - with Baffle, you can run these queries on encrypted data
    
-   Overcome the Legal, Compliance, and Risk concerns that may prevent moving to the cloud to realize the cost and convenience that AWS RDS and Aurora offer.
    
-   Overcome constant exceptions for security and compliance risks created because current encryption solutions are too expensive to implement and underperform on existing SLAs.
    
-   Avoid the costs, management, and time delays associated with implementing encryption in the applications.
    
-   Enable lower environments to develop and test with data that mirrors production data but is not a security risk because any sensitive data is obfuscated.
    
-   SaaS/multi-tenant providers can cryptographically isolate their tenant data and take advantage of the scale that SaaS provides. They can also ensure that their tenants' data is inaccessible to anybody (including the SaaS provider) who does not have the key.
