# What is GHPC?
GHPC is the core of Alces HPC/AI Stack Deployments.

Built on open tools and methodologies it is essential that GHPC gives back to the communities and industry that helped bring it together. The software and documentation here delivers everything needed to spin up a GHPC and customise it for your use case. 

HPC & AI stack deployment, configuration & maintenance can be a complex undertaking and often leads to bespoke solutions that are not portable and require expert maintenance. This project seeks to provide a platform that empowers the delivery of an industry-tested stack in a standardised manner.

## How it Works
GHPC provides 2 VM appliances that you can deploy on your chosen platform (e.g. libvirt, public cloud). These appliances provide the tools needed to create, deploy and manage multiple HPC & AI stacks ("clusters"). The Director & Domain sit in a "Domain Network", the director creates controllers. 

- **Director:** Configures, deploys & manages your clusters
- **Domain:** Identity server for user & host management
- **Controller:** The service provider for a cluster
- **Login:** End-user access to a cluster
- **Compute:** Resources for running workloads
