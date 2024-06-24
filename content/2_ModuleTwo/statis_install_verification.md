---
title: "Verify Policies" # MODIFY THIS TITLE IF APPLICABLE
chapter: true
weight: 2 
---

##  Verify Policy enrollment 


Navigate to Baffle Manager URL

![Capture-BMBreakdown](../images/image4_BM_Breakdown.png)
Figure 3. Baffle Manager Left Navigation

Check if below components are enrolled: 
-   Key Management -> Keystore -> aws-kms. This specifies AWS KMS and S3 location for storing the key encryption key (KEK) and data encryption key (DEK) respectively.

-   Key Management -> Key Encryption Keys -> alias/<stack_name>-baffle-shield-key. The KEK and related DEK are specified here.

-   Database Proxy -> Clusters -> proxy-_static_mask. This is where Baffle Shield is configured and programmed.
    
-   Database Proxies -> Databases -> PostgreSQL This is the target database that will contain the de-identified data

-   Database Proxies -> Data Sources-> ccn_dms_ds and ssn_dms_ds. This is the location of the social security numbers (ssn) and credit card numbers (ccn) to be protected. This includes the database, schema, table, column, and datatype.
    
-   Database Proxies -> Data Protection -> ccn-fpe-cc and ssn-fpe-decimal. This is the policy for encrypting ssn and ccn using format-preserving encryption. The ccn encryption has an additional step for ensuring the ciphertext passes the Luhn check.
    
