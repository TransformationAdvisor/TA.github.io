---
layout: post
title: How to exclude large files to improve Datacollector execution time.
categories: Datacollector
---

1. Find the customCmd.properties in the ./transformationadvisor-{version}/conf directory
2. Update the customCmd.properties file with the files wish to exclude e.g. --excludeFiles='.*/largeFile.xml'
3. Run the Data Collector
   Note: you will see in the logs file being excluded. 
