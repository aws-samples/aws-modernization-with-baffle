---
title: "Clean up"
chapter: true
weight: 7 
---

# Clean up

## Stop DMS

If you have finished Lab1, DMS is most likely stopped at the conclusion of Lab1. If you have continued to other Labs without finishing Lab1, Please make sure to stop DMS before cleaning up the environment.  

Go back to the DMS page and at the top right, click Actions->Stop to stop the DMS job. In the warning pop-up window, also click stop
If you are planning to work on the next LAB(s), Please do not clean up and continue to next LAB.


## Clean up

Navigate to the CloudFormation service. Click Stacks->(baffle-stack-name) and a window slides in from the right.

At the top of the new window, click Delete. A warning window will pop-up. Click Delete

If you used the CloudFormation template to create a new user group and policy, then delete that CF stack as well. That will delete the user group and policy, but not the user.
