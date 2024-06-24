---
title: "Load Data into Production" # MODIFY THIS TITLE IF APPLICABLE
chapter: true
weight: 4 
---

# Lab 1 - MASKING FOR DEV/TEST ENVIRONMENTS

## Load Data Into Production

Put sensitive ccn and ssn data into the production database, dms_source_db.

1.  Login into the AWS Console and navigate to the CloudFormation service. Click Stacks->(baffle-stack-name) and a window slides in from the right.
    
2.  Click the Outputs tab, Find and right click the link next to the PGAdmin URL  and select open in new tab. A new browser tab for pgAdmin will appear.
    
3.  In the new tab, log into pgAdmin using the credentials you created (same as Baffle Manager credentials). The pgAdmin dashboard should appear.
    
4.  On the left navigation pane called Object Explorer, there will be a database server called “dms_static_mask”, click on the chevron by it to show the different connections.
    
5.  Click the chevron next to direct@baffle and a window will pop-up. Enter the RDS database password you created earlier and click OK.
    
6.  Click the chevron next to Databases. Right click dms_source_db and then Query Tool. A new SQL command window will open on the right.
    
7.  Copy sample data into the customers table using these SQL commands:
  
    ```
       insert into customers (uuid, first_name, ccn, ssn, entity_id) values ('c19213ad-dff9-49a5-8ef0-fbe1964d6f66', 'Charley', '1111-1111-1111-1111', '111-11-1111', 1);
        
        insert into customers (uuid, first_name, ccn, ssn, entity_id) values ('1c598c48-2cda-4e70-99b1-a8adcc99a0a9', 'Cacilie', '2222-2222-2222-2222', '2222-22-2222', 2);
        
        insert into customers (uuid, first_name, ccn, ssn, entity_id) values ('bd58c5f8-5ca8-4a98-8a81-cf1575a7cf06', 'Jaye', '3333-3333-3333-3333', '333-33-3333', 3);
        
        insert into customers (uuid, first_name, ccn, ssn, entity_id) values ('bf9634a8-6d0b-4a4b-9658-5ca3d7a8679c', 'Katharyn', '4444-4444-4444-4444', '444-44-4444', 4);
        
        insert into customers (uuid, first_name, ccn, ssn, entity_id) values ('df59fe01-debe-4bd8-9b34-25d0469fab76', 'Val', '5555-5555-5555-5555', '555-55-5555', 5);
        
        insert into customers (uuid, first_name, ccn, ssn, entity_id) values ('7790c775-f4e1-40ca-94d9-3bc85f12ca09', 'Jefferson', '6666-6666-6666-6666', '666-66-6666', 6);
        
        insert into customers (uuid, first_name, ccn, ssn, entity_id) values ('dc23cb1a-62f1-4e2d-917f-92f24fd6578a', 'Merrielle', '7777-7777-7777-7777', '777-77-7777', 7);
        
        insert into customers (uuid, first_name, ccn, ssn, entity_id) values ('d1c12f3b-9895-478f-9f28-6773415e3373', 'Gilberte', '8888-8888-8888-8888', '888-88-8888', 8);
    

Click the “play” button to run the query.

  

8.  Verify the data is in the table with this command
    

          select * from customers;

Highlight that command and click the “play” button to run the query.

  
See the data in the data output pane below. Note these clear ccn and ssn values are highly contrived to be easy to identify.
Once the template creates the stack it will output some value.

