---
layout: post
title: Container of deployed application fails to start
categories: Migration
author: Kieran Geoghegan
---

Applications migrated to IBM Cloud Private may fail to start. In particular the container may fail to start and display the following event:

```
Failed to pull image "mycluster.icp:8500/namespace/app:a1927ad": rpc error: code = Unknown desc = Error response from daemon: Get https://mycluster.icp:8500/v2/namespace/app/manifests/a1927ad: unauthorized: authentication required

```

The problem is that the service account for the deployment is referring to an image pull secret that does not exist. To resolve the issue, create a secret called `sa-<target namespace>` for example `sa-microclimate-pipeline-deployments`

```
kubectl create secret docker-registry sa-microclimate-pipeline-deployments \
  --docker-server=<cluster_ca_domain>:8500 \
  --docker-username=<account-username> \
  --docker-password=<account-password> \
  --docker-email=<account-email> \
  --namespace=microclimate-pipeline-deployments

```
