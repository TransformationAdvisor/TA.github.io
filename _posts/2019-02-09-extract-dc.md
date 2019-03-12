---
layout: post
title: Checksum Error trying to extract the Data Collector
categories: Datacollector
author: John Cronin
meta: Appservers
---

if a checksum error is returned after trying to extract the Data Collector, Try this workaround


"gzip -d transformationadvisor-<OperatingSystem>_<workspace>_<collection>.tgz"

"tar xf transformationadvisor-<OperatingSystem>_<workspace>_<collection>.tar"
