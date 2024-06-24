---
title: "Add tenant data and verify encryption and tenant-access"
chapter: true
weight: 6 
---


## Add tenant data and verify encryption and tenant-access

Using pgAdmin, you will connect to the database directly to access the encrypted data.

1.  Login into the AWS Console and navigate to the CloudFormation service. Click Stacks->(baffle-stack-name) and a window slides in from the right.
    
2.  Click the Outputs tab, Find and right click the link next to the PGAdminURL  and select open in new tab. A new browser tab for pgAdmin will appear.
    
3.  In the new tab, log into pgAdmin using the same credentials you created above (same as Baffle Manager credentials). The pgAdmin dashboard should appear.
    
4.  On the left navigation pane called Object Explorer, there will be a server group “rle”. Click on the chevron by it to show the different connections.
    
5.  Click on the chevron next to direct@baffle to connect. Enter the RDS database password you created earlier.
    
6.  Under direct@baffle, click on the chevron by Databases to expand the menu below
    
7.  Right click rle_db and then Query Tool. An SQL command pane will open.
    
8.  Verify the data is in the table with this command
    
	```
	select * from customers;
	```
  Click the “play” button to run the query.


The table, “customers”, will appear. The ccn and ssn columns will indicate type bytea. The “entity_id” column defines which tenant is associated with the record. In this case, There are two tenants, “T-1001” and “T-2002” Each tenant has two users in this example. Though not possible to see here, each tenant is encrypted with its own key.

  

9.  To better see the encrypted values, enter this command:
    
	```
	select encode(ccn::bytea, 'hex'), encode(ssn::bytea, 'hex') from customers;
	```
 Highlight only that command and click the “play” button to run the query.
  

Note the entries all start with “baff1e…” And though it is not possible to see,

  

Note! Any direct database connection is now protected. Any DBA and/or cloud infrastructure administrator can do their jobs, but are not allowed to access the data. This “least privilege” aspect of data security and privacy is often overlooked. In addition to eliminating the risk of direct attacks on the database, Baffle allows outsourcing of the database management and allays any concerns of cloud migration.

  
  
  

## Connect to the Database Through Baffle Shield

Here we log into the database through Baffle Shield. This will allow you to read the data in the clear.

  

1.  In the left navigation pane called Object Explorer, click the chevron next to “rle” to expand the menu below it.
    
2.  Click the chevron next to shield@baffle to initiate a connection. Enter the RDS password that you provided earlier.
    
3.  Click the chevron next to “Databases” to expand the menu below it.
    
4.  Right click on rle_db and then Query Tool. A new SQL command pane opens.
    
5.  Use the following command to select all from the table “customers”
    
       ```
	SELECT * FROM customers;
       ```

   Click the “play” button to run the query.
  

Note that the ccn and ssn are displayed as “***”, indicating they are encrypted.

  

6.  Run this command to show all values in the table “customers” that belong to tenant “T-1001”

	```
	select * from customers where entity_id='T-1001';
	```

Highlight only that command and click the “play” button to run the query.

  

7.  Run this command to show John’s ccn
    
	```
	select ssn from customers where first_name='John' and entity_id='T-1001';
	```
  

Note! This example uses a column and the “where” command to identify the access and encryption keys. Baffle offers two additional methods, “Session” and “SQL comment”

  

In the “session” method, the tenants are mapped to the user (role) that the application uses to log in to the database. If the application logs in as the user mapped to tenant “T-1001” then only those records are decrypted during that session. The application must reconnect to the database with a different session with the user mapped to tenant “T-2002” to decrypt those respective records.

  

In the “SQL comment” method, the requesting application adds a specially crafted SQL comment that specifies the name of the tenant, so this command would return the entire table, but only decrypt tenant “T-1001” records:
	```
	select * from customers /*+ tenant:T-1001 */;
	```

##  Enter more data through Shield

1.  In the same SQL command pane from above, enter this data into table “customers”
    
	```
	insert into customers (uuid, first_name, ccn, ssn, entity_id) values ('5', 'Charley', '1111-1111-1111-1111', '111-11-1111', 'T-1001');
	insert into customers (uuid, first_name, ccn, ssn, entity_id) values ('6', 'Cacilie', '2222-2222-2222-2222', '2222-22-2222', 'T-2002');
	```

Highlight only those commands and click the “play” button to run the query.

2.  Highlight the ``` SELECT * FROM customers;``` command and click the “play” button to run the query and verify the two new entries.

3.  Highlight the ```select * from customers where entity_id='T-1001';``` command and click the “play” button to run the query and see only the T-1001 values.


##  Verify the New Data is Encrypted

1.  Along the top row of tabs, find and click the one that says “rle_db/baffle@direct@baffle*”

2.  Highlight the ```select encode(ccn::bytea, 'hex'), encode(ssn::bytea, 'hex') from customers;``` command and click the “play” button to run the query.
    

Verify the two new encrypted entries.

  
