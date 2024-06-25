---
title: "Dynamic Masking" # MODIFY THIS TITLE IF APPLICABLE
chapter: true
weight: 6 
---

## Dynamic Masking

Referring back to Figure 1, now we will use pg_Admin to connect to Baffle Shield to simulate the sensitive data access for Human Resources, Support, and the remote worker.

1.  Under dynamic-mask-db, click the chevron next to Shield@harry_HR. Then enter “harry” (no quotes) as the password
    
2.  Click the chevron next to dynamic-mask->Shield@harry_HR->Databases to expand the menu
    
3.  Under dynamic-mask->Shield@harry_HR->Databases, right click on dynamic_mask_db and then Query. This opens a new SQL query pane.
    
4.  Use the following command to select all from the table “customers”
    
    ```
      select * from customers;
    ```
    

Click the “play” button to run the query.

Notice that the ccn is all fully masked as “**confidential**” and the ssn is shown in the clear.

1.  Under dynamic-mask-db, click the chevron next to Shield@ron_Remote. Then enter “ron” (no quotes) as the password
    
2.  Click the chevron next to dynamic-mask->Shield@ron_Remote->Databases to expand the menu
    
3.  Under dynamic-mask->Shield@ron_Remote->Databases, right click on dynamic_mask_db and then Query. This opens a new SQL query pane.
    
4.  Use the following command to select all from the table “customers”
    
    ```
      select * from customers;
    ```
    

Click the “play” button to run the query.

Notice that both the ccn and ssn is all fully masked as “**confidential**”

1.  Under dynamic-mask-db, click the chevron next to Shield@sally_Support. Then enter “sally” (no quotes) as the password
    
2.  Click the chevron next to dynamic-mask->Shield@sally_Support->Databases to expand the menu
    
3.  Under dynamic-mask->Shield@sally_Support->Databases, right click on dynamic_mask_db and then Query. This opens a new SQL query pane.
    
4.  Use the following command to select all from the table “customers”
    
    ```
      select * from customers;
    ```
    

 Click the “play” button to run the query.

Notice that the ccn is partially masked where the first 12 digits are replaced with “X” and the last 4 are shown in the clear. The ssn is partially masked where the first 5 digits are randomly replaced with numbers and the last 4 are clear. If the select command is run again, new random numbers are generated for the masked values.
