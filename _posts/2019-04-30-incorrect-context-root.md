---
layout: post
title: Incorrect context root for web applications on WebSphere detected by DataCollector
categories: Datacollector
author: Philip Wu
---

For web applications running on WebSphere, in Transformation Advisor version 1.9.4 and later, the Data Collector detects the context root for the applications. The context root of the application is used in deployments to ICP on Liberty to simplify access to the application. If the context root of the application detected by the Data Collector is not appropriate, when accessing the deployed application, a page with the following error message is displayed: Context Root Not Found.

To workaround this problem, follow these steps to update the Helm chart for the application in the git repository:

1.  Clone the Git repository containing the Helm chart for the application
2. Edit the file chart/[application name]/values.yaml and change the rewriteTarget to "/" as shown below:
```
ingress:
  enabled: true
  rewriteTarget: "/"
```
3. Commit and push the changes to the Git repository
4. Wait for the application to be re-deployed. The ingress path for the application now redirects to the Liberty base page. The application can be accessed by appending the context root of the application to the ingress path. For example, the URL to access the application
 is "http://[ICP proxy IP]/modresorts/resorts/", if the ingress path is "modresorts" and the context root for the application is "resorts"