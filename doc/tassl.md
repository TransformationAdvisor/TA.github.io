---
layout: page
title: Configure TransformationAdvisor to use customer certificates
---

### These instructions assume you have TA installed with Ingress.

(1) Create a certificate and key. You can skip this step, if you already have a certificate and its key.

 + You can create a certificate and a key using this command: **openssl req -new -x509 -key ca.key -out ca.crt**
 + Here is an example defined the _commonName_ and _organisation_; Create a key called **ca.key**, and certificated called **ca.crt**:
 
{% highlight ruby %}
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
        -out ca.crt \
        -keyout ca.key \
        -subj "/CN=ta-dev-icp-proxy-1.rtp.raleigh.ibm.com/O=TA"
{% endhighlight %}

(2) Create a **tls** type secret: 
{% highlight ruby %}
kubectl create secret tls ta-tls-secret --key ca.key --cert ca.crt
{% endhighlight %}
 + Assumed you run the command at the same working directory of the ca.key and ca.crt files.
 + Create a secret named `ta-tls-secret`

(3) Add new return url to OIDC registration:

{% highlight ruby %}
curl -kv -X PUT -u oauthadmin:$OAUTH2_CLIENT_REGISTRATION_SECRET -H 
"Content-Type: application/json" --data @oidc-registration.json 
https://$ICP_MASTER_IP:9443/oidc/endpoint/OP/registration/$TA_CLIENT_ID
{% endhighlight %}
 + You can get your TA's OIDC file by running: 
 {% highlight ruby %}
 curl -kv -X GET -u oauthadmin:$OAUTH2_CLIENT_REGISTRATION_SECRET -H 
 "Content-Type: application/json" 
 https://$ICP_MASTER_IP:9443/oidc/endpoint/OP/registration/$TA_CLIENT_ID
{% endhighlight %}
 + In short, is to add `https://$YOUR_DOMAIN_DEFINED_IN_COMMON_NAME:443` and similar suffix/path to each of the fields having the proxy IP.
 + 433 port is significant, unless you mapped 443 to other ports in your ICP.
 + OAUTH2_CLIENT_REGISTRATION_SECRET can be get from: 
 {% highlight ruby %}
 kubectl get secret platform-oidc-credentials -o yaml -n kube-system
{% endhighlight %}

 The value of OAUTH2_CLIENT_REGISTRATION_SECRET is base64 encoded, thus, you need to decode: **base64 --decode <<< value**

{% highlight ruby %}
{
  "token_endpoint_auth_method": "client_secret_basic",
  "client_id": "$TA_CLIENT_ID",
  "client_secret": "$TA_CLIENT_SECRET",
  "scope": "openid profile email",
  "grant_types": [
    "authorization_code",
    "client_credentials",
    "password",
    "implicit",
    "refresh_token",
    "urn:ietf:params:oauth:grant-type:jwt-bearer"
  ],
  "response_types": [
    "code",
    "token",
    "id_token token"
  ],
  "application_type": "web",
  "subject_type": "public",
  "post_logout_redirect_uris": [
    "https://$TA_INGRESS_IP:443/$TA_RELEASE_NAME-ui/logout",
    "https://$YOUR_DOMAIN_DEFINED_IN_COMMON_NAME:443/$TA_RELEASE_NAME-ui/logout"
    "https://$TA_MASTER_IP:8443/console/logout",
    "https://mycluster.icp:8443/console/logout"
  ],
  "preauthorized_scope": "openid profile email general",
  "introspect_tokens": true,
  "trusted_uri_prefixes": [
   "https://$YOUR_DOMAIN_DEFINED_IN_COMMON_NAME:443/",
    "https://$TA_INGRESS_IP:443/"
  ],
  "redirect_uris": [
    "https://$YOUR_DOMAIN_DEFINED_IN_COMMON_NAME:443/$TA_RELEASE_NAME-ui/icp/auth/callback",
    "https://$TA_INGRESS_IP:443/$TA_RELEASE_NAME-ui/icp/auth/callback"
  ]
}
{% endhighlight %}
 + The example below use _ta-dev-icp-proxy-1.rtp.raleigh.ibm.com_ as the domain name; _ta-test_ as TA's helm release name; 
 _9.42.29.103_ has Ingress IP; _9.42.23.152_ as master IP.
 
{% highlight ruby %}
{
  "token_endpoint_auth_method": "client_secret_basic",
  "client_id": "$TA_CLIENT_ID",
  "client_secret": "$TA_CLIENT_SECRET",
  "scope": "openid profile email",
  "grant_types": [
    "authorization_code",
    "client_credentials",
    "password",
    "implicit",
    "refresh_token",
    "urn:ietf:params:oauth:grant-type:jwt-bearer"
  ],
  "response_types": [
    "code",
    "token",
    "id_token token"
  ],
  "application_type": "web",
  "subject_type": "public",
  "post_logout_redirect_uris": [
    "https://9.42.29.103:443/ta-test-ui/logout",
    "https://ta-dev-icp-proxy-1.rtp.raleigh.ibm.com:443/ta-test-ui/logout"
    "https://9.42.23.152:8443/console/logout",
    "https://mycluster.icp:8443/console/logout"
  ],
  "preauthorized_scope": "openid profile email general",
  "introspect_tokens": true,
  "trusted_uri_prefixes": [
   "https://ta-dev-icp-proxy-1.rtp.raleigh.ibm.com:443/",
    "https://9.42.29.103:443/",
    "http://localhost:3000/"
  ],
  "redirect_uris": [
    "https://ta-dev-icp-proxy-1.rtp.raleigh.ibm.com:443/ta-test-ui/icp/auth/callback",
    "https://9.42.29.103:443/ta-test-ui/icp/auth/callback"
  ]
}
{% endhighlight %}

(4) Backup TA's Ingress yaml. You can run **kubectl get ingress ta-test-ibm-transadv-dev-ingress -o yaml**, copy and paste the results to a text file. 
  + Assumed you have TA helm release called **ta-test**, then your TA's Ingress is **ta-test-ibm-transadv-dev-ingress**. You can always find it at **kubectl get ingress**
  + Note: indention is significant. 
(5) Delete the TA's Ingress yaml. [Read the note below before proceeding] **kubectl delete ingress ta-test-ibm-transadv-dev-ingress**.
  + **NOTE: you can try to edit the ingress without deleting it: **kubectl edit ingress ta-test-ibm-transadv-dev-ingress** but it gives inconsistent results i.e. failed to swap certificate most of the time.
  
(6) If you deleted the Ingress, then update the ingress file, you backed up at step 4:
  + Added **host** under **rules**
  + Added **tls** under **spec**
  + The host value shall be the same as the **commonName** in the certificate stored in the secret. 
  + In my test environment, I did not make **IP** worked as **commonName** and **host**
  + The example below, I have host value of **ta-dev-icp-proxy-1.rtp.raleigh.ibm.com** and from secrete **ta-tls-secret**
```
spec:
  rules:
  - host: ta-dev-icp-proxy-1.rtp.raleigh.ibm.com
    http:
      paths:
      - backend:
          serviceName: ta-test-ibm-transadv-dev-ui
          servicePort: 3000
        path: /ta-test-ui
      - backend:
          serviceName: ta-test-ibm-transadv-dev-server
          servicePort: 9080
        path: /ta-test-server
  tls:
  - hosts:
    - ta-dev-icp-proxy-1.rtp.raleigh.ibm.com
    secretName: ta-tls-secret
```

(7) Save the ingress file and go to the file directory, and run. **kubectl apply -f $PATH_TO_INGRESS_FILE/ta-igress.yaml**
  + The example I saved the file named **ta-igress.yaml**
  + Here is an example absolute path (**$PATH_TO_INGRESS_FILE/ta-igress.yaml**) **/Users/ibm/Downloads/ta-igress.yaml** pointing to the file
(8) Test the ingress: **kubectl get ingress**

  + If you see, **HOSTS** with value of *****, you need to add load balancer to the ingress.
  
```
NAME                               HOSTS     ADDRESS       PORTS     AGE
ta-test-ibm-transadv-dev-ingress   *         9.42.29.103   80, 443   2m
```

(9) Add load balancer. Run **kubectl edit ingress ta-test-ibm-transadv-dev-ingress**, and
 + Add/edit to the example below
 + Sample IP 9.42.29.103 is the proxy IP in ICP
```
status:
  loadBalancer:
    ingress:
    - ip: 9.42.29.103
```
(10) Save and exit, re-run: **kubectl get ingress**, you shall see the **HOSTS** now points to your domain:

```
NAME                               HOSTS                                    ADDRESS       PORTS     AGE
ta-test-ibm-transadv-dev-ingress   ta-dev-icp-proxy-1.rtp.raleigh.ibm.com   9.42.29.103   80, 443   56s
```

(11) Test certificate on the Ingress itself: 
{% highlight ruby %}
curl -v -k --resolve $YOUR_DOMAIN_DEFINED_IN_COMMON_NAME:443:$LOAD_BALANCER_IP 
https://ta-dev-icp-proxy-1.rtp.raleigh.ibm.com
{% endhighlight %}

 + Example: 
 {% highlight ruby %}
 curl -v -k --resolve ta-dev-icp-proxy-1.rtp.raleigh.ibm.com:443:9.42.29.103 
 https://ta-dev-icp-proxy-1.rtp.raleigh.ibm.com
 {% endhighlight %}
 + In the example: **ta-dev-icp-proxy-1.rtp.raleigh.ibm.com** is my domain at port 443 pointing to **9.42.29.103**
 + In the output, you shall see ***subject: CN=ta-dev-icp-proxy-1.rtp.raleigh.ibm.com; O=TA**

```
* Added ta-dev-icp-proxy-1.rtp.raleigh.ibm.com:443:9.42.29.103 to DNS cache
* Rebuilt URL to: https://ta-dev-icp-proxy-1.rtp.raleigh.ibm.com/
* Hostname ta-dev-icp-proxy-1.rtp.raleigh.ibm.com was found in DNS cache
*   Trying 9.42.29.103...
* TCP_NODELAY set
* Connected to ta-dev-icp-proxy-1.rtp.raleigh.ibm.com (9.42.29.103) port 443 (#0)
* ALPN, offering h2
* ALPN, offering http/1.1
* Cipher selection: ALL:!EXPORT:!EXPORT40:!EXPORT56:!aNULL:!LOW:!RC4:@STRENGTH
* successfully set certificate verify locations:
*   CAfile: /etc/ssl/cert.pem
  CApath: none
* TLSv1.2 (OUT), TLS handshake, Client hello (1):
* TLSv1.2 (IN), TLS handshake, Server hello (2):
* TLSv1.2 (IN), TLS handshake, Certificate (11):
* TLSv1.2 (IN), TLS handshake, Server key exchange (12):
* TLSv1.2 (IN), TLS handshake, Server finished (14):
* TLSv1.2 (OUT), TLS handshake, Client key exchange (16):
* TLSv1.2 (OUT), TLS change cipher, Client hello (1):
* TLSv1.2 (OUT), TLS handshake, Finished (20):
* TLSv1.2 (IN), TLS change cipher, Client hello (1):
* TLSv1.2 (IN), TLS handshake, Finished (20):
* SSL connection using TLSv1.2 / ECDHE-RSA-AES256-GCM-SHA384
* ALPN, server accepted to use h2
* Server certificate:
*  subject: CN=ta-dev-icp-proxy-1.rtp.raleigh.ibm.com; O=TA
*  start date: Mar 19 15:51:20 2019 GMT
*  expire date: Mar 18 15:51:20 2020 GMT
*  issuer: CN=ta-dev-icp-proxy-1.rtp.raleigh.ibm.com; O=TA
*  SSL certificate verify result: self signed certificate (18), continuing anyway.
* Using HTTP2, server supports multi-use
* Connection state changed (HTTP/2 confirmed)
* Copying HTTP/2 data in stream buffer to connection buffer after upgrade: len=0
* Using Stream ID: 1 (easy handle 0x7fb4f8806a00)
> GET / HTTP/2
> Host: ta-dev-icp-proxy-1.rtp.raleigh.ibm.com
> User-Agent: curl/7.54.0
> Accept: */*
> 
* Connection state changed (MAX_CONCURRENT_STREAMS updated)!
< HTTP/2 404 
< server: nginx/1.15.6
< date: Tue, 19 Mar 2019 16:07:23 GMT
< content-type: text/plain; charset=utf-8
< content-length: 21
< strict-transport-security: max-age=15724800; includeSubDomains
< 
* Connection #0 to host ta-dev-icp-proxy-1.rtp.raleigh.ibm.com left intact
```
12. You can also tested on UI app:
<img width="1020" alt="screenshot 2019-03-20 at 0 08 05" src="https://media.github.ibm.com/user/8257/files/e0eb7800-4aa8-11e9-9de9-917f6f01b6c8">
