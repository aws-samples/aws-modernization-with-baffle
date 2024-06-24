---
title: "Verify Installation"
chapter: true
weight: 2 
---
## Verify Tenant Enrollment

Navigate to Baffle Manger URL

Check if below components, tenants and Encryption policies are setup:

-   Key Management -> Tenants -> Rle-Tenant-1, Rle-Tenant-2 maps tenants to their encryption keys
    
-   Key Management -> Key Encryption Keys -> alias/<baffle-stack-name>-baffle-shield-key-1, alias/<baffle-stack-name>-baffle-shield-key-2, and alias/<baffle-stack-name>-baffle-shield-key-3. The KEK and related DEK are specified for each of the tenants in this example.
    
-   Key Management -> Keystore -> aws-kms. This specifies AWS KMS and S3 location for storing the key encryption key (KEK) and data encryption key (DEK) respectively. In this example, all three tenants are using the same keystore, but in production, they would each provide their own.
    
-   Database Proxies -> Databases -> PostgreSQL This is the database that will contain the protected data
    
-   Database Proxies -> Data Protection ->rle-ssn-ccn-dpp. This is the policy for encrypting ssn and ccn using traditional AES deterministic encryption.
    
-   Database Proxies -> Data Sources-> ccn_rle_ds and ssn_rle_ds. This is the location of the social security numbers (ssn) and credit card numbers (ccn) to be protected. This includes the database, schema, table, column, and datatype of each.
    
-   Database Proxy -> Clusters -> proxy-rle. This is where Baffle Shield is configured and programmed.

![Capture-BMBreakdown](../images/image4_BM_Breakdown.png)
Figure 3. Baffle Manager Left Navigation
