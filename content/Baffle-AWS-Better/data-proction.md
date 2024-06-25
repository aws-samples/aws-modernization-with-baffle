---
title: "Data Protection in Modern Environments"
chapter: true
weight: 1 
---

## Data Protection in Modern Environments
Moving to the cloud offers cost benefits, but security concerns hold some back. Encryption helps, but proper implementation is key. PostgreSQL, a powerful open-source database, lacks built-in transparent data encryption (TDE). Even with TDE, physical security isn't the main concern. CISOs worry about remote access breaches due to stolen credentials, software vulnerabilities, and misconfigurations. Legal and Risk teams need assurances the cloud provider can't access sensitive data. Here's where "least privilege" access control comes in. It minimizes access to data, ensuring only authorized users can see what they need, significantly reducing the attack surface for remote breaches. Encryption and least privilege are crucial for securing cloud-based data and ensuring confidentiality.

As architects look to maintain a competitive advantage and leverage AWS’s suite of innovative platforms, they face headwinds related to ever-evolving security frameworks and compliance regulations focused on risk mitigation.

Data encryption is the gold standard for de-risking sensitive data and preventing exposure from breaches.

  -  Why haven’t organizations adopted encryption more widely?
 
    

-   Application code changes are required to implement “Data-in-Use” protection. Major revisions to the code are required to integrate the tool, address gaps in query support, and migrate the data to a protected state.
    
-   Application Performance concerns - Assuming the encrypt/decrypt process will lead to poor end-user experience and potentially violate existing SLAs.
  - The inability to process encrypted data - Gaining utility from encrypted data is simple; you just need to decrypt all of it and run your analytics, creating another compromise in data security.
