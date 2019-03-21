---
layout: page
title: Configuring Ingress to use your own CA certificate instead of TA's Ingress certificate
---

### These instructions assume you have TA installed with Ingress.

(1) Add new return url to OIDC registration.


`curl -kv -X PUT -u oauthadmin:$OAUTH2_CLIENT_REGISTRATION_SECRET -H "Content-Type: application/json" --data @oidc-registration.json https://$MASTER_IP:9443/oidc/endpoint/OP/registration/$ur-client-id-defined-at-TA-installation | jq .`

Sample json file to be to sent:

 + In short, is to add `https://ta-dev-icp-proxy-1.rtp.raleigh.ibm.com:443` to each of the fields having the proxy IP.
 + 433 port is significant.
 + OAUTH2_CLIENT_REGISTRATION_SECRET can be get from: 
 
 `kubectl get secret platform-oidc-credentials -o yaml -n kube-system`
 
 The value of OAUTH2_CLIENT_REGISTRATION_SECRET is base64 encoded, thus, you need to decode: 
 
 `base64 --decode <<< value`
 

```
{
  "token_endpoint_auth_method": "client_secret_basic",
  "client_id": "ur-client-id-defined-at-TA-installation",
  "client_secret": "ur-client-secret-defined-at-TA-installation",
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
    "https://9.42.29.103:443/ken-test-9-ui/logout",
    "https://ta-dev-icp-proxy-1.rtp.raleigh.ibm.com:443/ken-test-9-ui/logout"
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
    "https://ta-dev-icp-proxy-1.rtp.raleigh.ibm.com:443/ken-test-9-ui/icp/auth/callback",
    "https://9.42.29.103:443/ken-test-9-ui/icp/auth/callback"
  ]
}
```

(2) Backup TA's Ingress yaml. You can run `kubectl get ingress ken-test-9-ibm-transadv-dev-ken-ingress -o yaml` , copy and paste the results to a text file. Note: indention is significant.

(3) Delete the TA's Ingress yaml. [Read the note below before proceeding] `kubectl delete ingress ken-test-9-ibm-transadv-dev-ingress`

  + **NOTE: you can try to edit the ingress without deleting it: `kubectl edit ingress ken-test-9-ibm-transadv-dev-ken-ingress` but our test environment's Ingress failed to swap certificate most of the time.**
  
(4) If you deleted the Ingress, then update the ingress file, you backed up at step 2:

  + added `host` under `rules`
  + added `tls` under `spec`
  + the host value shall be the same as the `commonName` in the certificate stored in the secret. The example below, I have host value of `ta-dev-icp-proxy-1.rtp.raleigh.ibm.com` and from secrete `ken-tls-secret`
```
spec:
  rules:
  - host: ta-dev-icp-proxy-1.rtp.raleigh.ibm.com
    http:
      paths:
      - backend:
          serviceName: ken-test-9-ibm-transadv-dev-ken-ui
          servicePort: 3000
        path: /ken-test-9-ui
      - backend:
          serviceName: ken-test-9-ibm-transadv-dev-ken-server
          servicePort: 9080
        path: /ken-test-9-server
  tls:
  - hosts:
    - ta-dev-icp-proxy-1.rtp.raleigh.ibm.com
    secretName: ken-tls-secret
```

(5) Save the ingress file and go to the file directory, and run. `kubectl apply -f /Users/ibm/Downloads/ken-igress.yaml`
  + the example I saved the file named `ken-igress.yaml`
  + I ran the command from directory `/Users/ibm/Downloads/`
  
(6) Test the ingress: `kubectl get ingress`
  + if you see, `HOSTS ` with value of `*`, you need to add load balancer to the ingress.
  
```
NAME                                  HOSTS     ADDRESS       PORTS     AGE
ken-test-9-ibm-transadv-dev-ingress   *         9.42.29.103   80, 443   2m
```

(7) Add load balancer. Run `kubectl edit ingress ken-test-9-ibm-transadv-dev-ken-ingress`, and
 + add/edit to the example below
 + 9.42.29.103 is the proxy IP
 
```
status:
  loadBalancer:
    ingress:
    - ip: 9.42.29.103
```

(8) Save and exit, re-run: `kubectl get ingress`, you shall see the `HOSTS` now points to your domain:

```
NAME                                      HOSTS                                    ADDRESS       PORTS     AGE
ken-test-9-ibm-transadv-dev-ken-ingress   ta-dev-icp-proxy-1.rtp.raleigh.ibm.com   9.42.29.103   80, 443   56s
```

(9) Test certificate on the Ingress itself: 

`curl -v -k --resolve ta-dev-icp-proxy-1.rtp.raleigh.ibm.com:443:9.42.29.103 https://ta-dev-icp-proxy-1.rtp.raleigh.ibm.com`
 + `ta-dev-icp-proxy-1.rtp.raleigh.ibm.com` is my domain pointing to `9.42.29.103`
 + In the output, you shall see ***  subject: CN=ta-dev-icp-proxy-1.rtp.raleigh.ibm.com; O=TA**


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

(10) You can also tested on UI app:

<img width="1020" alt="screenshot 2019-03-20 at 0 08 05" src="https://media.github.ibm.com/user/8257/files/e0eb7800-4aa8-11e9-9de9-917f6f01b6c8">
