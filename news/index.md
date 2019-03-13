---
layout: page
title: What's New
---

**February 14th, 2019** Transformation Advisor 1.9.3 released.

	- Data Collector Enhancements
	- Data Collector collects in English language locale to resolve locale based issues
	- MQ producers are now detected and matched for WAS applications

**February 4th, 2019** Transformation Advisor 1.9.2 released.

	- Data Collector Enhancements
	- Static and dynamic dependency information is now collected
	- Analysis Enhancements
	- Analysis includes the relationships between Applications and Shared Libraries
	- Analysis includes the relationships between Applications and MQ QueueManagers
	- Migration Enhancements
	- Required Shared Libraries are identified during migration
	- Binaries can be sourced from a Maven repository during migration
	- Migrated applications use the latest CloudPak Liberty Helm charts

**December 14, 2018** Transformation Advisor 1.9.1 released.

     - Configurable heap sizes for analysis of large systems
     - Support for -java-home parameter
     - Support for execution as user other than wsadmin owner
     - Improved handling of SOAP Timeout issues
     - Improved robustness of handling outside file locations
     - Improved handling of unexpected files during upload

**November 21, 2018** Transformation Advisor 1.9.0 released.

    - IBM MQ
       See what it will take to migrate to IBM Cloud Private
       See summary of migration complexity for each Queue Manager  
       See details of all migration issues including suggested solution   
    - Experimental Mode
       Introducing experimental mode!  
       Press Ctrl + Shift + X to see experimental dev effort values for MQ     
    - Shared Libraries on WebSphere
       Shared Library information is now collected for each profile
       Inventory report for each app contains shared libraries being used
       Shared libraries files are now available in the recommendations screen   
    - Data Collector Enhancements
       Defaulting to use system Java (non-Windows systems) to reduce issues  
       Support for custom profiles
       Support for scanning jar and zip files 
       Support for custom location of profileRegistry.xml
       Improved logging for better trouble shooting
    - Export
       PDF/CSV export allows application and summary information to be exported
    
**October 26, 2018** Transformation Advisor 1.8.1 released.

    - Updated Liberty image included 
    
**October 3, 2018** Transformation Advisor 1.8.0 released.

    - Beta feature for MQ Queue Manager Analysis via Data Collector
    - Support for installation into non-default namespace of IBM Cloud Private 3.1
    - Integration with MicroClimate v1.6.0
    - Quick starter guide for WebSphere Traditional on Private Cloud

**September 6, 2018** Transformation Advisor 1.7.2 released.

    - Fix to complexity determination

**August 24, 2018** Transformation Advisor 1.7.1 released.

    - Integration with MicroClimate v1.5.0

**August 23, 2018** Transformation Advisor 1.7.0 released.

    - JBoss and WebLogic Analysis
    - Instructions for migrating transformed applications to IBM Kubernetes Service
    - Usability Enhancements
    - Data Collector Enhancements
    - Workspace/collection name allowed characters change

**June 29th, 2018** Transformation Advisor 1.6.0 released.

    - User Authentication
    - Application specific server.xml
    - TA logs integrated with ICP Logging

**June 6th, 2018** Transformation Advisor 1.5.1 released.

    - Fixes issue with working with latest Microclimate v1.2.0

**May 18th, 2018** Transformation Advisor 1.5.0 released.

    - Microclimate integration. 
    - The recommendations page has been re-designed
    - Ingress supported
    - Support for running TA on ICP on Z

**March 13th, 2018** Transformation Advisor 1.4.0 released.

    - Migration Bundles - TA automatically generates files deploy your app on ICP.
    - Classification of recommendations into Simple/Moderate/Complex
    - New Welcome page
    - More intuitive user experience using workspaces and collections to manage your work
    - Numerous other UX Improvements
    - Runs on ICP on Power platform

**Feb 7th, 2018** Transformation Advisor 1.3.0 released.

    - Extended the Data Collector agent platform support
    - Screen flow improved 
    - Recommendations now shows dependencies separately to issues
    - Dev costs can be configured by the customer to reflect their capabilities

**Jan 15th, 2018** Transformation Advisor 1.2.0 released.

    - Collect JMS and Database connection info
    - Create server.xml for migrating Liberty applications
    - Scan EARS/WARS outside running WAS instance
    - Drop DC results directly onto UI for disconnected usecase

**Nov 28th, 2017** Transformation Advisor 1.1.0 released.

    - Data Collector supported on additional platforms - AIX, Windows, Solaris
    - UI improvements based on feedback
    - Noise reduction - Integrated latest binary scanner (17.0.0.3)
    - Build and deploy automation

**Oct 24th, 2017** Transformation Advisor 1.0.0 released.

    - Initial release of tool as free content on ICP
