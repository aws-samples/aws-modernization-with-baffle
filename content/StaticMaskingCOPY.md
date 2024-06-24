---
title: "Lab 1 - Masking for Dev/Test Environments" # MODIFY THIS TITLE IF APPLICABLE
chapter: true
weight: 2
---


[Baffle Guide for AWS Demo for Lower Environment (Static Masking) ]{.c31
.c0 .c36}

[]{.c7}

[Baffle is the easiest way to protect data.]{.c7}

# [Static Masking]{.c31 .c10 .c35} {#static-masking .c4}

[Static masking is a process of removing sensitive data from a database
that can't be recovered. In some cases, this might be accomplished with
partial or full masking.  However, in this example, encryption will be
used and the encrypted data will be considered "destroyed" in the
lower-environment because there is no access to the encryption
keys.]{.c7}

[The purpose of this demonstration is to simulate simultaneously copying
and de-identifying production data to a lower environment in real-time.
 Refer to figure 1.  ]{.c7}

[![image](/Users/lnutakki/Downloads/aws-modernization-with-baffle/content/2_ModuleTwo/images/image2.png){style="width: 624.00px; height: 252.00px; margin-left: 0.00px; margin-top: 0.00px; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px);"}]{style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 624.00px; height: 252.00px;"}

[Figure 1. Setup for Static Masking]{.c30}

[It is assumed that there is a trusted production environment with an
AWS RDS database and application.  The production database is operating
with data in the clear (though it should be noted that the Baffle
product could be placed between the application and database to encrypt
data even in the production database).  Development and testing
engineers need data in their environments that closely resembles the
quantity and type of data in production.  However, the sensitive data
can't be used in these lower environments without significantly
increasing security and privacy risks.  Baffle is used to  implement
format preserving encryption (FPE) to de-identify the data as AWS DMS
copies from the production database to the database in the lower
environment.  The advantage of FPE over traditional encryption is that
the ciphertext is the same datatype and length of the original clear
text, thus application and human testing is not affected.  The
de-identified data in the lower environment is updated in real time if
DMS is set for continuous backup.]{.c7}

[Note that this demonstration also simulates sharing data with business
partners and/or migration to cloud environments where some (or all) of
the data is sensitive and must be de-identified.]{.c7}

[Prerequisites:]{.c7}

1.  [A browser that can connect to your AWS environment.]{.c7}
2.  [AWS Console access to an AWS environment with the ability to deploy
    Cloudformation, AWS RDS, AWS DMS, and an EC2. ]{.c7}

<!-- -->

1.  [If using an Admin, then skip the "AWS IAM setup" and continue to
    "Setup the AWS Infrastructure". ]{.c7}
2.  [If you prefer to create a user specifically for this demo, Baffle
    has created a CloudFormation template to set policy and a user
    group.  Start with "AWS IAM Setup"]{.c7}

## [AWS IAM Setup]{.c22 .c10} {#h.qr5h61qr5n08 .c9}

1.  [Use CloudFormation (CF) to create the user group and
    policies.]{.c7}

<!-- -->

1.  [Download the "create_group_role_template.yaml" file from this
    location:
    ]{.c10}[[baffle-public/market-place/cloudformation-template/lower-environment/create_group_policy
    at master · baffle/baffle-public
    (github.com)](https://www.google.com/url?q=https://github.com/baffle/baffle-public/tree/master/market-place/cloudformation-template/lower-environment/create_group_policy&sa=D&source=editors&ust=1718067218801138&usg=AOvVaw1jJ0OdHfvSLg5IGNaFnCX9){.c26}]{.c25
    .c10}
2.  [As an AWS ]{.c10}[Admin]{.c32 .c10}[, log in to the AWS console and
    go to the CloudFormation service]{.c7}
3.  [Click ]{.c10}[Stacks]{.c0}[-\>]{.c10}[Create
    Stack]{.c0}[-\>]{.c10}[With new resources(standard)]{.c0}
4.  [Select ]{.c10}[Choose and existing template]{.c0}[ then
    ]{.c10}[Upload a template file]{.c0}[ then ]{.c10}[Choose
    file]{.c0}[ and navigate to the file.  Select the file and click
    ]{.c10}[Open. ]{.c0}[Click ]{.c10}[Next]{.c0}
5.  [In ]{.c10}[Stack Name]{.c10 .c18}[, enter a name for the stack.
    This will also become the name of the resulting user group, with
    "-group" appended.  ie \<iam-stack-name\>-group. Click
    ]{.c10}[Next]{.c0}
6.  [Leave all defaults. Click ]{.c10}[Next]{.c0}
7.  [Under ]{.c10}[Capabilities]{.c10 .c18}[, check the box
    acknowledging that IAM resources will be created. Click
    ]{.c10}[Submit]{.c0}

<!-- -->

2.  [Create a user and attach to the user group.]{.c7}

<!-- -->

1.  [As an AWS ]{.c10}[Admin]{.c10 .c32}[, log in to the AWS console and
    go to the IAM service]{.c7}
2.  [Go to the IAM services and click ]{.c10}[User groups]{.c0}[.
     Ensure the new user group is there.]{.c7}
3.  [Click ]{.c10}[Users]{.c0}[-\>]{.c10}[Create User]{.c0}[.  Enter a
    User Name.  Check the box to provide user access to the AWS
    management console. ]{.c7}
4.  [Select the method you prefer for setting the password for console
    access and click ]{.c10}[Next]{.c0}[.  ]{.c7}
5.  [In ]{.c10}[Permissions Option]{.c10 .c18}[s, select]{.c10}[ Add
    user to group]{.c0}[ and then below that, check the box that
    corresponds to the group created above. Click
    ]{.c10}[Next]{.c0}[.]{.c7}
6.  [Click ]{.c10}[Create user]{.c0}
7.  [Note the password or download the csv file.  Click ]{.c10}[Return
    to users list]{.c0}

## [Setup the AWS infrastructure]{.c10 .c29} {#h.xvayfjcntr7z .c9}

[A CloudFormation (CF) template will be used to create AWS
infrastructure that simulates Figure 1.  This includes an RDS Postgres
instance with two logical databases in it. One called "dms_source_db"
and one called "dms_target_db" which represent production and
development databases, respectively. A DMS instance will be created that
will copy from the production database to the development database.
 Baffle software consists of Baffle Manager and Baffle Shield.  Baffle
Shield is the reverse proxy that does the encryption as DMS moves the
data.  Baffle Manager will program Baffle Shield.  Secrets Manager will
store the database credentials.  An S3 bucket will store the Data
Encryption Key (DEK) and AWS KMS will store the key encryption key (KEK)
which is a customer managed key (CMK) in AWS terms.  Finally, both
applications will be represented by a pgAdmin connection.  ]{.c7}

1.  [Download the "create_baffle_workflows_template.yaml" from this
    location:
    ]{.c10}[[baffle-public/market-place/cloudformation-template/create_baffle_workflows_template.yaml
    at master · baffle/baffle-public
    (github.com)](https://www.google.com/url?q=https://github.com/baffle/baffle-public/blob/master/market-place/cloudformation-template/create_baffle_workflows_template.yaml&sa=D&source=editors&ust=1718067218804133&usg=AOvVaw3jUKdbFHOyr20pBM1hXcQl){.c26}]{.c10
    .c25}
2.  [Either as an AWS admin or the user created in the "AWS IAM Setup"
    section above, log into the AWS console and go to the CloudFormation
    service.]{.c7}
3.  [Click ]{.c10}[Stacks]{.c0}[-\>]{.c10}[Create
    Stack]{.c0}[-\>]{.c10}[With new resources(standard)]{.c0}
4.  [Select ]{.c10}[Choose an existing template]{.c0}[ then
    ]{.c10}[Upload a template file]{.c0}[ then ]{.c10}[Choose
    file]{.c0}[ and navigate to the file.  Select the file and click
    ]{.c10}[Open. ]{.c0}[Click ]{.c10}[Next]{.c0}
5.  [In ]{.c10}[Stack Name]{.c10 .c18}[, enter a name for the stack, we
    will refer to it as \<baffle-stack-name\>]{.c7}
6.  [In ]{.c10}[Parameters]{.c10 .c18}

<!-- -->

1.  [Workflow]{.c10 .c18}[ - Select STATIC_MASK from the dropdown for
    this demo.  (This same CF template can create several demos).]{.c7}
2.  [UserEmail]{.c10 .c18}[ - create a username (email suggested) for
    Baffle Manager and pgAdmin]{.c7}
3.  [UserPassword]{.c10 .c18}[- create a password for Baffle Manager and
    pgAdmin]{.c7}
4.  [DBPassword]{.c10 .c18}[ - create a password for the RDS database
    that will be created]{.c7}
5.  [MyIP]{.c10 .c18}[ - enter the IPv4 address of your location which
    can be found at
    ]{.c10}[[https://checkip.amazonaws.com](https://www.google.com/url?q=https://checkip.amazonaws.com&sa=D&source=editors&ust=1718067218806143&usg=AOvVaw0wXl4fMt1YldVzMpxfGvT9){.c26}]{.c10
    .c24}[ This is to add you to the security group so you can access
    the AWS infrastructure that this CF template will create.]{.c7}
6.  [Take note of all the credentials as they will be needed later.
    Click ]{.c10}[Next]{.c0}

<!-- -->

7.  [Leave the defaults.  Click]{.c10}[ Next]{.c0}
8.  [Under ]{.c10}[Capabilities]{.c10 .c18}[, check the box
    acknowledging that IAM resources might be created (though none will
    be). Click ]{.c10}[Submit]{.c0}[ ]{.c10}[        ]{.c16}

[Note! It can take up to 20 minutes for this CF template to run. This
demo can't continue until the stack status = CREATE_COMPLETE]{.c10 .c37}

[]{.c7}

## [Launch Baffle Manager]{.c22 .c10} {#h.9eq28ofu6tv5 .c9}

1.  [Login into the AWS Console and navigate to  the CloudFormation
    service. Click
    ]{.c10}[Stacks]{.c0}[-\>]{.c10}[(baffle-stack-name)]{.c0}[ and a
    window slides in from the right.]{.c7}
2.  [Click the ]{.c10}[Outputs]{.c0}[ tab, Find and right click the link
    next to the ]{.c10}[BaffleManagerURL ]{.c15}[and select "open in new
    tab".  A new browser tab for Baffle Manager will appear.]{.c8}
3.  [ In the new tab, click through the privacy warnings because a
    self-signed certificate was used. In production, proper certificates
    and trust would have to be established.]{.c7}
4.  [Log in with the Baffle Manager credentials you provided
    above.]{.c7}

[]{.c7}

[![A screenshot of a login form Description automatically
generated](/Users/lnutakki/Downloads/aws-modernization-with-baffle/content/2_ModuleTwo/images/image1.png){style="width: 539.50px; height: 403.07px; margin-left: 0.00px; margin-top: 0.00px; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px);"}]{style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 539.50px; height: 403.07px;"}

[Figure 2. Baffle Manager Login Page]{.c30}

[Once logged in, note the left navigation pane (Figure 3).  This is
where all processes start.  The first section is Database Proxies and is
the focus of this demonstration.  The next section is for managing the
Data Proxy, which is a separate Baffle product. Just note that several
sub-headers have the same name in both the Database and Data proxy
sections, so make sure to use the Data]{.c10}[base]{.c32
.c10}[ section.]{.c7}

[For this demonstration, Baffle Manger has already been programmed
through our API.  However, we will step through the artifacts
here.]{.c7}

[![A screenshot of a computer Description automatically
generated](/Users/lnutakki/Downloads/aws-modernization-with-baffle/content/2_ModuleTwo/images/image4.png){style="width: 453.35px; height: 424.34px; margin-left: 0.00px; margin-top: 0.00px; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px);"}]{style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 453.35px; height: 424.34px;"}

[Figure 3. Baffle Manager Left Navigation]{.c30}

[Check to ensure CloudFormation created the following: ]{.c7}

-   [Key Management -\> Key Encryption Keys -\>
    alias/\<stack_name\>-baffle-shield-key. The KEK and related DEK are
    specified here.]{.c7}
-   [Key Management -\> Keystore -\> aws-kms. This specifies AWS KMS and
    S3 location for storing the key encryption key (KEK) and data
    encryption key (DEK) respectively. ]{.c7}
-   [Database Proxies -\> Databases -\> PostgresSQL This is the target
    database that will contain the de-identified data ]{.c7}
-   [Database Proxies -\> Data Protection -\> ccn-fpe-cc and
    ssn-fpe-decimal. This is the policy for encrypting ssn and ccn using
    format-preserving encryption. The ccn encryption has an additional
    step for ensuring the ciphertext passes the Luhn check.]{.c7}
-   [Database Proxies -\> Data Sources-\> ccn_dms_ds and ssn_dms_ds.
    This is the location of the social security numbers (ssn) and credit
    card numbers (ccn)  to be protected. This includes the database,
    schema, table, column, and datatype.]{.c7}
-   [Database Proxy -\> Clusters -\> proxy-\_static_mask.  This is where
    Baffle Shield is configured and programmed.]{.c7}

[]{.c7}

## [Turn DMS on]{.c22 .c10} {#h.1cldscwc69ik .c9}

[Trigger DMS to start copying any  data in the production database
(dms_source_db) to the lower environment database (dms_target_db) on a
continual basis]{.c7}

[]{.c7}

1.  [Login into the AWS Console and navigate to  the CloudFormation
    service. Click
    ]{.c10}[Stacks]{.c0}[-\>]{.c10}[(baffle-stack-name)]{.c0}[ and a
    window slides in from the right.]{.c10}
2.  [Click the ]{.c10}[Outputs]{.c0}[ tab, Find and right click the link
    next to the ]{.c10}[DMSMigrationTaskURL]{.c0}[ ]{.c15}[and select
    "open in new tab".  A new browser tab for DMS will appear.]{.c8}
3.  [In the top right of the new tab, click
    ]{.c10}[Actions]{.c0}[-\>]{.c10}[Restart/Resume]{.c0}[  ]{.c7}

[]{.c7}

## [Load Data Into Production]{.c22 .c10} {#h.ufp1tfg7lpn .c9}

[Put sensitive ccn and ssn data into the production database,
dms_source_db.]{.c7}

1.  [Login into the AWS Console and navigate to  the CloudFormation
    service. Click
    ]{.c10}[Stacks]{.c0}[-\>]{.c10}[(baffle-stack-name)]{.c0}[ and a
    window slides in from the right.]{.c10}
2.  [Click the ]{.c10}[Outputs]{.c0}[ tab, Find and right click the link
    next to the ]{.c10}[PGAdminURL]{.c8 .c18}[ ]{.c15}[and select
    ]{.c8}[open in new tab]{.c15}[.  A new browser tab for pgAdmin will
    appear.]{.c8}
3.  [In the new tab, log into pgAdmin using the credentials you created
    (same as Baffle Manager credentials). The pgAdmin dashboard should
    appear.]{.c8}
4.  [On the left navigation pane called ]{.c8}[Object Explorer]{.c8
    .c18}[, will be a database server called "dms_static_mask", click on
    the chevron by it to show the different connections.]{.c8 .c31}
5.  [Click the chevron next to ]{.c8}[direct@baffle]{.c15}[ and a window
    will pop-up.  Enter the RDS database password you created earlier
    and click ]{.c8}[OK]{.c15}[.]{.c8 .c31}
6.  [Click the chevron next to ]{.c10}[Databases]{.c0}[. R]{.c10}[ight
    click ]{.c8}[dms_source_db]{.c15}[ and then ]{.c8}[Query Tool.
    ]{.c15}[A new SQL command window will open on the right.]{.c8}
7.  [Copy sample data into the customers table using these SQL
    commands:]{.c7}

[]{.c7}

[insert into customers (uuid, first_name, ccn, ssn, entity_id) values
(\'c19213ad-dff9-49a5-8ef0-fbe1964d6f66\', \'Charley\',
\'1111-1111-1111-1111\', \'111-11-1111\', 1);]{.c2}

[insert into customers (uuid, first_name, ccn, ssn, entity_id) values
(\'1c598c48-2cda-4e70-99b1-a8adcc99a0a9\', \'Cacilie\',
\'2222-2222-2222-2222\', \'2222-22-2222\', 2);]{.c2}

[insert into customers (uuid, first_name, ccn, ssn, entity_id) values
(\'bd58c5f8-5ca8-4a98-8a81-cf1575a7cf06\', \'Jaye\',
\'3333-3333-3333-3333\', \'333-33-3333\', 3);]{.c2}

[insert into customers (uuid, first_name, ccn, ssn, entity_id) values
(\'bf9634a8-6d0b-4a4b-9658-5ca3d7a8679c\', \'Katharyn\',
\'4444-4444-4444-4444\', \'444-44-4444\', 4);]{.c2}

[insert into customers (uuid, first_name, ccn, ssn, entity_id) values
(\'df59fe01-debe-4bd8-9b34-25d0469fab76\', \'Val\',
\'5555-5555-5555-5555\', \'555-55-5555\', 5);]{.c2}

[insert into customers (uuid, first_name, ccn, ssn, entity_id) values
(\'7790c775-f4e1-40ca-94d9-3bc85f12ca09\', \'Jefferson\',
\'6666-6666-6666-6666\', \'666-66-6666\', 6);]{.c2}

[insert into customers (uuid, first_name, ccn, ssn, entity_id) values
(\'dc23cb1a-62f1-4e2d-917f-92f24fd6578a\', \'Merrielle\',
\'7777-7777-7777-7777\', \'777-77-7777\', 7);]{.c2}

[insert into customers (uuid, first_name, ccn, ssn, entity_id) values
(\'d1c12f3b-9895-478f-9f28-6773415e3373\', \'Gilberte\',
\'8888-8888-8888-8888\', \'888-88-8888\', 8);]{.c2}

[]{.c2}

[Click the "play" button
]{.c10}[![image](/Users/lnutakki/Downloads/aws-modernization-with-baffle/content/2_ModuleTwo/images/image3.png){style="width: 28.00px; height: 25.00px; margin-left: 0.00px; margin-top: 0.00px; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px);"}]{style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 28.00px; height: 25.00px;"}[to
run the query.]{.c7}

[]{.c7}

8.  [Verify the data is in the table with this command]{.c7}

[            ]{.c7}

[select \* from customers;]{.c2}

[Highlight that command and click the "play" button
]{.c10}[![image](/Users/lnutakki/Downloads/aws-modernization-with-baffle/content/2_ModuleTwo/images/image3.png){style="width: 28.00px; height: 25.00px; margin-left: 0.00px; margin-top: 0.00px; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px);"}]{style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 28.00px; height: 25.00px;"}[to
run the query.]{.c7}

[]{.c7}

[See the data in the data output pane below. Note these clear ccn and
ssn values are highly contrived to be easy to identify.]{.c7}

[]{.c7}

## [Observe De-Identified Data]{.c22 .c10} {#h.elrauhyvn87b .c9}

[Once DMS senses new data in production, it copies that data over to the
lower environment, dsm_target_db through Baffle Shield. At this time,
Baffle Shield will de-identify ccn and ssn with format preserving
encryption.]{.c7}

[]{.c7}

1.  [Back in the ]{.c10}[Object Explorer]{.c10 .c18}[,  Under
    ]{.c10}[dms_static-mask-\>direct@baffle-\>Databases, right click
    ]{.c8}[dms_target_db]{.c15}[ and then ]{.c8}[Query Tool. ]{.c15}[A
    new SQL command pane opens.]{.c8}
2.  [Use the following command to select all from the table
    "customers".]{.c7}

[]{.c7}

[SELECT \* FROM customers;]{.c2}

[]{.c7}

[Click the "play" button
]{.c10}[!image](/Users/lnutakki/Downloads/aws-modernization-with-baffle/content/2_ModuleTwo/images/image3.png){style="width: 28.00px; height: 25.00px; margin-left: 0.00px; margin-top: 0.00px; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px);"}]{style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 28.00px; height: 25.00px;"}[to
run the query.]{.c7}

[]{.c7}

[Observe that the ssn and ccn of each user is encrypted.  Because the
encryption is format preserving, it may not not normally be obvious, but
since the clear numbers were all easy to see (ie 111-11-1111), it is now
easy to see the encrypted values are very different.]{.c7}

[]{.c7}

## [(Optional) Show Continuous Updates]{.c22 .c10} {#h.kjuh4qjpyhv .c9}

[This is just to show that any future production changes are captured by
DMS and updated but de-identified in the lower environment.  These
ongoing updates are an advantage over many other approaches to static
masking that have to be done periodically and can be disruptive.]{.c7}

1.  [Go back to the Query tool for the production (sales) database.
     Along the top see the tab that says
    "dms_source_db/baffle@direct@baffle\*"]{.c7}
2.  [Delete a the entry for Charley and enter a new row for Horace by
    entering this command:]{.c7}

[delete from customers where first_name = \'Charley\';]{.c2}

[insert into customers (uuid, first_name, ccn, ssn, entity_id) values
(\'8ac6425d-9895-478f-9f28-b37841ac39ee\', \'Horace\',
\'9999-9999-9999-9999\', \'999-99-9999\', 9);]{.c2}

[]{.c2}

[Highlight only that command and click the "play" button
]{.c10}[![image](/Users/lnutakki/Downloads/aws-modernization-with-baffle/content/2_ModuleTwo/images/image3.png){style="width: 28.00px; height: 25.00px; margin-left: 0.00px; margin-top: 0.00px; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px);"}]{style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 28.00px; height: 25.00px;"}[to
run the query.]{.c10}

[]{.c2}

3.  [Go back to the Query tool for the lower environment (sales_dev)
    database.  Along the top see the tab that says
    "dms_target_db/baffle@direct@baffle\*"]{.c7}
4.  [Select all from the table "customers".]{.c7}

[]{.c7}

[SELECT \* FROM customers;]{.c2}

[]{.c7}

[Click the "play" button
]{.c10}[![image](/Users/lnutakki/Downloads/aws-modernization-with-baffle/content/2_ModuleTwo/images/image3.png){style="width: 28.00px; height: 25.00px; margin-left: 0.00px; margin-top: 0.00px; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px);"}]{style="overflow: hidden; display: inline-block; margin: 0.00px 0.00px; border: 0.00px solid #000000; transform: rotate(0.00rad) translateZ(0px); -webkit-transform: rotate(0.00rad) translateZ(0px); width: 28.00px; height: 25.00px;"}[to
run the query.]{.c10}

[        ]{.c2}

[Observe that Charley was deleted and Horace added with a de-identified
ccn and ssn.]{.c7}

[]{.c7}

## [Delete the CloudFormation Entries to Release the Resources]{.c10 .c22} {#h.ebjz5gjzdh59 .c9}

1.  [Go back to the DMS page and at the top right, click
    ]{.c10}[Actions]{.c0}[-\>]{.c10}[Stop]{.c0}[ to stop the DMS job. In
    the warning pop-up window, also click ]{.c10}[stop]{.c14 .c0}
2.  [Navigate to the CloudFormation service. Click
    ]{.c10}[Stacks]{.c0}[-\>]{.c10}[(baffle-stack-name)]{.c0}[ and a
    window slides in from the right.]{.c7}
3.  [At the top of the new window, click ]{.c10}[Delete]{.c0}[. A
    warning window will pop-up.  Click ]{.c10}[Delete]{.c0}
4.  [If you used the CloudFormation template to create a new user group
    and policy, then delete that CF stack as well.  That will delete the
    user group and policy, but not the user.]{.c7}

[]{.c14 .c33}
