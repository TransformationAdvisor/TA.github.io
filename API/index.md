---
layout: page
title: APIs
---
IBM Cloud Transformation Advisor provides access to it's full capabilities via a set of standard REST APIs.    
The APIs are compliant with OpenAPI specification 3.0    
	
	
## API documentation and interactive ui
The swagger documentation for the APIs are available from your IBM Cloud Transformation Advisor install at this location: `<TA_SERVER>/api/`   
An interactive UI for these APIs can be found at: `<TA_SERVER>/api/explorer/`

#### Finding the <TA_SEVER> value
The `<TA_SERVER>` value can be found from the IBM Cloud Private UI using the following steps:
1. Launch IBM Cloud Private Console UI
1. Navigation to the IBM Cloud Transformation Advisor Helm release
1. The 'launch' action will provide two options - select the server
1. This URL is the `<TA_SERVER>` value

#### Limitations of 'Try it out'
When running IBM Cloud Transformation Advisor in IBM Cloud Private with ingress enabled the 'Try it out' capability will fail on execution.    
This is because the generated curl command is missing the ingress value.    
This value can be added to the command, and then the command run manually to test the API.    


## API key creation	
To access the APIs you will need to use an API Key, the process for creating this can be found [here](./APIKey_creation_in_ICP.md)

## API REST client
To easily integrate IBM CloudTransformation Advisor APIs into your product or tool an open source TA REST client is being developed.    
More details to follow shortly.
