---
title: "Verify Encryption in DB" # MODIFY THIS TITLE IF APPLICABLE
chapter: true
weight: 5 
---
## Verify the Data was Encrypted in the Database (Least Privilege)

Here we log directly into the database, still as the business owner, “baffle”, without Baffle Shield as a proxy.

1.  Navigate to dynamic-mask->direct@baffle. Click the chevron next to direct@baffle and enter the RDS database credential from earlier.
    
2.  Click the chevron next to Databases
    
3.  Right click on dynamic_mask_db and then Query Tool. A new query pane opens.
    
4.  Use the following command to select all from the table “customers” and observe that the SSN of each user is encrypted.
    
    ```
      select * from customers;
    ```
    

Click the “play” button to run the query.

Because the encryption is format preserving, it may not always be obvious, but the ccn and ssn digits are no longer all the same number.

Note! Any direct database connection is now protected. So not only this business owner, but any DBA and/or cloud infrastructure administrator can do their jobs, but are not allowed to access the data. This “least privilege” aspect of data security and privacy is often overlooked. In addition to eliminating the risk of direct attacks on the database, Baffle allows outsourcing of the database management and allays any concerns of cloud migration.
