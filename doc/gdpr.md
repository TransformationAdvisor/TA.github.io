---
layout: page
title: GDPR Guidance
---

# IBM Cloud Transformation Advisor considerations for GDPR readiness
 
___________________________________________________________________________________________________
### For PID(s): UT:30AHB

## Notice:

This document is intended to help you in your preparations for GDPR readiness. It provides information about features of IBM Cloud Product Insights Transformation Advisor that you can configure, and aspects of the product’s use, that you should consider to help your organization with GDPR readiness. This information is not an exhaustive list, due to the many ways that clients can choose and configure features, and the large variety of ways that the product can be used in itself and with third-party applications and systems.

__Clients are responsible for ensuring their own compliance with various laws 
and regulations, including the European Union General Data Protection Regulation. 
Clients are solely responsible for obtaining advice of competent legal counsel as to 
the identification and interpretation of any relevant laws and regulations that may 
affect the clients’ business and any actions the clients may need to take to comply 
with such laws and regulations.__

__The products, services, and other capabilities 
described herein are not suitable for all client situations and may have restricted 
availability. IBM does not provide legal, accounting, or auditing advice or represent or 
warrant that its services or products will ensure that clients are in compliance with 
any law or regulation.__


___________________________________________________________________________________________________

### Table of Contents
1. GDPR
2. Product Configuration for GDPR
3. Data Life Cycle
4. Data Collection
5. Data Storage
6. Data Access
7. Data Processing
8. Data Deletion
9. Data Monitoring

___________________________________________________________________________________________________
### GDPR

General Data Protection Regulation (GDPR) has been adopted by the European Union ("EU") and applies from May 25, 2018.

#### Why is GDPR important?

GDPR establishes a stronger data protection regulatory framework for processing of personal data of individuals.  GDPR brings:

* New and enhanced rights for individuals
* Widened definition of personal data
* New obligations for processors
* Potential for significant financial penalties for non-compliance
* Compulsory data breach notification

#### Read more about GDPR

* (EU GDPR Information Portal)[https://www.eugdpr.org/]
* (ibm.com/GDPR website)[http://ibm.com/GDPR]

___________________________________________________________________________________________________
### Product Configuration - considerations for GDPR Readiness
#### Offering Configuration

The following sections provide considerations for configuring IBM Cloud Product Insights Transformation Advisor to help your organization with GDPR readiness.


___________________________________________________________________________________________________
### Data Life Cycle

IBM Cloud Product Insights Transformation Advisor is an application that runs on the IBM Cloud Private platform. It's purpose is to aid developers in modernizing their application portfolio. To that end it gathers and processes meta data about Java Enterprise applications running on IBM WebSphere Application Servers, and creates recommendations on how best these applications can be changed to run in a container on a Cloud platform.

As such, the IBM Cloud Product Insights Transformation Advisor application deals primarily with technical data that is related to Java programming structures and associated application configuration data, and does not include the gathering or processing of personal data.  This data is persisted on the IBM Cloud Private platform in a database. Documentation on IBM Cloud Private platform can be found in the IBM Cloud Private collection in IBM Knowledge Center. (https://www-03preprod.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/kc_welcome_containers.html?view=kc)


#### Data Collection

The IBM Cloud Product Insights Transformatuion Advisor tool does not collect personal data. It does process technical data such as IP addresses which might be considered personal data. All such information is only accessible by the system administrator through a management console with role-based access control or by the system administrator through login to an IBM Cloud Private platform node.

The IBM Cloud Product Insights Transformatuion Advisor tool also reaquests the user for a Git username and password/token to enable the tool to push the artifacts it generates to the Github repository. This data is not stored and must be re-entered on each use. The data is transmitted to Git and is encrypted in transit.

This is not a definitive list of the types of data that are collected by the IBM Cloud Product Insights Transformatuion Advisor tool. It is provided as an example for consideration. If you have any questions about the types of data, contact IBM.

___________________________________________________________________________________________________
### Data Storage

The IBM Cloud Product Insights Transformatuion Advisor tool persists technical data in stateful stores on local or remote file systems as files or in a couch database. Consideration must be given to securing all data at rest. The IBM Cloud Product Insights Transformatuion Advisor tool supports encryption of data at rest in stateful stores that use dm-crypt. For more information, see Encrypting volumes by using dm-crypt (https://www-03preprod.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/installing/etcd.html?view=kc)

The following items highlight the areas where data is stored, which you might want to consider for GDPR.

**Application Data:** IBM Cloud Product Insights Transformatuion Advisor uses CouchDB as a backing data store to persist the technical data that is related to Java programming recommendations and associated application configuration data. CouchDB uses the underlying GlusterFS on which it is deployed for storage. Consideration must be given to encrypting the volumes where GlusterFS storage is deployed. For more information, see Encrypting volumes by using dm-crypt (https://www-03preprod.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/installing/etcd.html?view=kc)

**Logging Data:** Some technical data such as the IP address of the users browser may be stored in access logs. IBM Cloud Private platform uses an ELK stack for system logs. ELK is an abbreviation for three products, Elasticsearch, Logstash, and Kibana, that are built by Elastic and together comprise a stack of tools that you can use to stream, store, search, and monitor logs. The ELK stack that is provided with IBM Cloud Private platform uses the official ELK stack images that are published by Elastic. Logging is configured by default for the IBM Cloud Private platform services.  

**User Authentication Data, including User IDs and passwords:** This type of data is not stored by IBM Cloud Product Insights 
___________________________________________________________________________________________________
### Data Access

IBM Cloud Product Insights Transformatuion Advisor Application Data contains no personal information and is accessible through a Web user interface. Access to this user interface is not controlled.

IBM Cloud Product Insights Transformatuion Advisor Logging data access is controlled by the IBM Cloud Private access control features and can be accessed through the following defined set of interfaces.

Kubernetes kubectl CLI

IBM Cloud Private CLI

Helm CLI


These interfaces are designed to allow Administration access to IBM Cloud Private and can be secured involving three logical, ordered stages when a request is made: authentication, role-mapping, and authorization


**Authentication**

The IBM Cloud Private platform authentication manager accepts user credentials from the management console and forwards the credentials to the backend OIDC provider, which validates the user credentials against the enterprise directory. The OIDC provider then returns an authentication cookie (auth-cookie) with the content of a JSON Web Token (JWT) to the authentication manager. The JWT token persists information such as the user ID and email address, in addition to group membership at the time of the authentication request. This authentication cookie is then sent back to the management console. The cookie is refreshed during the session. It is valid for 12 hours after you sign out of the management console or close your web browser.


For all subsequent authentication requests made from the management console, the front-end NGINX server decodes the available authentication cookie in the request and validates the request by calling the authentication manager.


The IBM Cloud Private platform CLI requires the user to provide credentials to log in.


The kubectl CLI also requires credentials to access the cluster. These credentials can be obtained from the management console and expire after 12 hours. Access through service accounts is supported.

Helm CLI access utilizes certificates to access the cluster. Client certificates contain personal information, such as the user ID and email address, which are stored on cluster in MongoDB.

**Role Mapping**

IBM Cloud Private platform supports role-based access control (RBAC). In the role mapping stage, the user name that is provided in the authentication stage is mapped to a user or group role. The roles are used when authorizing which administrative activities can be carried out by the authenticated user.

**Authorization**

IBM Cloud Private platform roles control access to cluster configuration actions, to catalog and Helm resources, and to Kubernetes resources. Several IAM (Identity and Access Management) roles are provided, including Cluster Administrator, Administrator, Operator, Editor, Viewer. A role is assigned to users or user groups when you add them to a team. Team access to resources can be controlled by name-space.


**Pod Security**

Pod security policies are used to set up cluster-level control over what a pod can do or what it can access. For more information, see the iBM Cloud Private GDPR readiness documentation for further details (https://www-03preprod.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/getting_started/gdpr_readiness.html)

___________________________________________________________________________________________________
### Data Processing

Users of IBM Cloud Product Insights Transformation Advisor can control the way that data is processed and secured through system configuration.

**Pod security policies** are used to set up cluster-level control over what a pod can do or what it can access.

**Data-in-transit** is protected by using TLS and IPSEC. HTTPS (TLS underlying) is used for secure data transfer between user client and back end services. Users can specify the root certificate to use during installation. All inter-node data traffic can be encrypted out of the box by using IPSEC without changing any applications.

**Data-at-rest** protection is supported by using dm-crypt to encrypt data.


___________________________________________________________________________________________________
### Data Deletion

IBM Cloud Private platform provides commands, application programming interfaces (APIs), and user interface actions to delete data that is created or collected by IBM Cloud Product Insights Transformation Advisor. For more information, see the iBM Cloud Private GDPR readiness documentation for further details (https://www-03preprod.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/getting_started/gdpr_readiness.html)

The data retention period for logging data (ELK) is configurable.

Logging data can be deleted from the ELK stack by using Elasticsearch APIs.

All technical data that is related to platform configuration can be deleted through the management console or the Kubernetes kubectl API.

___________________________________________________________________________________________________
### Data Monitoring

IBM Cloud Private platform provides a monitoring service to monitor the status of your cluster and applications. This service uses Grafana and Prometheus to present detailed information about cluster nodes and containers. Monitoring can be configured to generate alerts or integrated with external alert providers. Platform monitoring is enabled by default. Additional monitoring stacks can be deployed for monitoring IBM CLoud Product Insights Transformation Advisor running on IBM Cloud Private.. For more information, see the iBM Cloud Private GDPR readiness documentation for further details (https://www-03preprod.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/getting_started/gdpr_readiness.html)
___________________________________________________________________________________________________
