This repository maintains deep dive on how the core of SteppingCloud Product platform works.

# Base Archicture of Multi-Tenancy for SteppingCloud products

Hi, Let's start with the awesome journey of System desing and archicture and how we started multi-tenancy for all our products.

Building a SaaS Multi-Tenant Application requires a different thinking approach compnared to non multi-tenant Applications. We will break the functionality into independent standalone services ,these services will further break into microservices. We have taken the distributed SAGA pattern for our archicture reference.

## Master Services Layer

This service is act as a master service, steppingcloud will manage and use this for tenant provisioning as well as for furure product service activation for a particular tenant.

SteppingCloud will have access to all the mertic and data generated by all the out tenants.

### **_Functionality_**

- Register Company Name to Master's Database.
- Assign a TenantID to Registered Company.
- Keep Status of number of Services subscribed by a tenant and expire date.
- Ability to unsubscribe a partcular service of a teannt.
- Create SFTP dev and test server for new tenant.
- Create database Tenants on production database cluster.
- Create Mapping of SFTP, TenantID in Master Database

## Tenant Services Layer

Tenant Services will act as a classifier for an incoming request with respect to each namespace[product]. Each service has only one ability/funcationality, its will be shared by all the product that will be build in future on top of this archicture.

By using multi tenant archicture we can auto-scale that service, this also gives is the capability for us to be a polyglot environment without distubing other services in the ecosystem.

### **_Current Universal Functionality_**

- Login Service
  - For tentantID and auth
- SuccessFactors odata Service
  - Native integration with SF
- Admin Service
  - Admin Control for Customers

### Tenant Services for below Products

#### AlumniHub

A hub for Alumni to connect and also change for companies to rehire best guys they once lost.

- Login/Authentication service.
- Admin Control Panel service.
- odata APIs service for Carrers, Terminated employees, SF Employee Profile.
- SFTP service for Admin Manual data uplaod and saving Salary Slips, other documents.

#### ErgoProxy

When AI meets HR domain, its a virtual personal digital assistant that do actions on your behalf if you ask him.

- Login/Authentication service.
- Admin Control Panel service.

## Tenant Database Layer

At Database layer we are using mongoDB, inorder for multi-tenancy, Master service will create a database per tenant and database may have multipule collections based on services they have subscribed.

### Database security

- We have saperated tenant by assigning a saperate database.
- Each database is encrypted at rest and in transit. In order to access the database Tenant service layer have to send auth token of repective tenant they want to connect. This information will be saved in Master service Layer.

### Database Horizontal and vertical scaling

- For Scaling we are using managed services by ATLAS, all this will be managed by atlas itself.
