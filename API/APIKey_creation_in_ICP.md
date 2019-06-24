---
layout: page
title: API key creation in IBM Cloud private
---

When IBM Cloud Transformation Advisor is running in IBM Cloud Private it is recommended that you have ingress enabled.    
With ingress enabled all API calls will require authentication.    
It is recommended that you create an API Key to use for all calls to the IBM Cloud Transformation Advisor APIs. 

The steps below outline how to create an API Key:  
1. Configure your client to connect to your ICP
1. Get your access token by executing the following command
    - `curl -k -H "Content-Type: application/x-www-form-urlencoded;charset=UTF-8" -d "grant_type=password&username=<USER>&password=<PWD>&scope=openid" https://<ICP_MASTER_IP>:8443/idprovider/v1/auth/identitytoken`
    - The username and password are the same as those you use to log into IBM Cloud Private
    - A JSON object will be returned. Take only the value defined for `access_token`
1. Set the access toekn into a variable
    - `ACCESS_TOKEN=<access_token>`
1. Now get your service ids using the following command
    - `curl -k -X GET -H "Accept: application/json" -H "Authorization: Bearer $ACCESS_TOKEN" https://<ICP_MASTER_IP>:8443/iam-token/serviceids/`
    - A JSON object will be returned. Take the first `crn` value in the object
    - The value should look like this `crn:v1:icp:private:iam-identity:mycluster:n/kube-system::serviceid:ServiceId-94ad5cb0-46e9-475f-a8c3-ba4638255dee`
1. Create an API key bound to your service id
    - `curl -k -X POST -H "Content-Type: application/json" -H "Accept: application/json" -H "Authorization: Bearer $ACCESS_TOKEN" -d '{"name": "test_serviceid_apikey","description": "Description for test_serviceid_apikey ","boundTo": "<crn_value>"}' https://<ICP_MASTER_IP>:8443/iam-token/apikeys/`
1. The JSON object returned will have an attribute called `apiKey`
1. This `apiKey` value is used in the apiKey field for all IBM Cloud Transformation Advisor API calls


