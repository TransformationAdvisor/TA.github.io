---
layout: page
title: BETA APIs
---
IBM Cloud Transformation Advisor provides access to it's capabilities via a set of standard REST APIs.    
The APIs are compliant with OpenAPI specification 3.0    
	
## BETA release
The APIs provided here are not yet fully complete and are subject to change.    
There is no guarantee that future release will support these APIs (IBM Cloud Transformation Advisor is not yet backward compatible in relation to APIs).     
The good news is that we can and will make changes for you!
	
## API documentation and interactive ui
The swagger documentation for the APIs are available from your IBM Cloud Transformation Advisor install at this location: `<TA_SERVER>/api/`   
An interactive UI for these APIs can be found at: `<TA_SERVER>/api/explorer/`

### Finding the `<TA_SEVER>` value    
The `<TA_SERVER>` value can be found from the IBM Cloud Private UI using the following steps:
1. Launch IBM Cloud Private Console UI
1. Navigation to the IBM Cloud Transformation Advisor Helm release
1. The 'launch' action will provide two options - select the server
1. This URL is the `<TA_SERVER>` value

### Limitations of 'Try it out'    
When running IBM Cloud Transformation Advisor in IBM Cloud Private with ingress enabled the 'Try it out' capability will fail on execution.    
This is because the generated curl command is missing the ingress value.    
This value can be added to the command, and then the command run manually to test the API.    

### Finding the `<TA_SEVER>` value with IBM Cloud Transformation Advisor Local   
When using IBM Cloud Transformation Advisor Local (available [here](https://www.ibm.com/cloud/garage/practices/learn/ibm-transformation-advisor)) the process of finding the `<TA_SERVER>` value is different
1. From the location where you run IBM Transformation Advisor Local, run this command: `docker ps`
1. Look for the image with 'server' in the name and check the value of the port
    - The default value is 2220, as can be seen in this exmaple: `9443/tcp, 0.0.0.0:2220->9080/tcp`
1. Go this this url `<TA_LOCAL_UI_URL>:<SERVER_PORT>/api/explorer/`

## API key creation	
To access the APIs you will need to use an API Key, the process for creating this can be found [here](./APIKey_creation_in_ICP.md)

## API REST client
To easily integrate IBM Cloud Transformation Advisor APIs into your product or tool an open source REST client is being developed. More details to follow shortly
