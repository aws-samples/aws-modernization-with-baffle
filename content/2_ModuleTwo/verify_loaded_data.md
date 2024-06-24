---
title: "Verify Migration and Encryption" # MODIFY THIS TITLE IF APPLICABLE
chapter: true
weight: 4 
---

# Lab 1 - MASKING FOR DEV/TEST ENVIRONMENTS

## Verify the DMS migration and masked in the target db 

Once DMS senses new data in production, it copies that data over to the lower environment, dsm_target_db through Baffle Shield. At this time, Baffle Shield will de-identify ccn and ssn with format preserving encryption.

  

1.  Back in the PDADMIN Object Explorer, Under dms_static-mask->direct@baffle->Databases, right click dms_target_db and then Query Tool. A new SQL command pane opens.
    
2.  Use the following command to select all from the table “customers”.
    

  

        SELECT * FROM customers;

  

    Click the “play” button to run the query.

  
Observe that the ssn and ccn of each user is encrypted. Because the encryption is format preserving, it may not not normally be obvious, but since the clear numbers were all easy to see (ie 111-11-1111), it is now easy to see the encrypted values are very different.

### Verify Continuous Updates
This is just to show that any future production changes are captured by DMS and updated but de-identified in the lower environment.  These ongoing updates are an advantage over many other approaches to static masking that have to be done periodically and can be disruptive.

1. Go back to the Query tool for the production database.  Along the top see the tab that says “dms_source_db/baffle@direct@baffle*”

2. Delete a the entry for Charley and enter a new row for Horace by entering this command:


    delete from customers where first_name = 'Charley';
    insert into customers (uuid, first_name, ccn, ssn, entity_id) values ('8ac6425d-9895-478f-9f28-b37841ac39ee', 'Horace', '9999-9999-9999-9999', '999-99-9999', 9);

Highlight only that command and click the “play” button to run the query.

3. Go back to the Query tool for the lower environment (sales_dev) database.  Along the top see the tab that says “dms_target_db/baffle@direct@baffle*”
Select all from the table “customers”.




        SELECT * FROM customers;



    Click the “play” button to run the query.


Observe that Charley was deleted and Horace added with a de-identified ccn and ssn.

