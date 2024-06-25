---
title: "Why Baffle"
chapter: true
weight: 3
---
# Why Baffle

At the heart of the Baffle solution is Baffle Shield, a reverse proxy placed between the application and database that performs encryption, access control, data policy enforcement, and key management. It operates at the network level so that the application does not know it exists, which is how the database can be encrypted without application code changes. 

The Baffle Manager configures and monitors all the Baffle Shields deployed in the organization.  It provides GUI and API interfaces for ease of use and automation options.

Baffle is not a SaaS; rather, Baffle Shield and Baffle Manager are delivered as containers that can be deployed to mirror the database deployments for high-availability and disaster recovery architectures.  Start with one instance and horizontally scale up with demand using a container management system such as Kubernetes 

