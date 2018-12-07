---
layout: post
title: Running data collector gives a "libjvm.so failed to load:” error
categories: Datacollector
author: Niall Horgan
---

Java can not find the correct libraries. You should use the Java on the machine itself instead of the one that comes down with the DC. Here are the things that need to be done first:

* 1.	Check if wsadmin is owned/run by a specific user. If YES then the same user should be used to run the Data Collector
* 2.	Make sure that the user above has read, write and execute permissions to the location where you have exploded the DC

Now you need to set up to use the machines Java (the one that the WAS is using)

* 1.	cd into transformationadvisor-2.1
* 2.	Replace the jre directory with the jre directory from the WAS machine itself, that WAS is using
* 3.	Run the Data Collector command again
