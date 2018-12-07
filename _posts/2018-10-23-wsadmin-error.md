---
layout: post
title: wsadmin.sh could not connect to the selected profile
categories: Datacollector
---

For wsadmin.sh to work the WAS system has to be running. Please check that it is. Also check that you are using the correct credentials. You can check your credentials using this command to see if it connects : 

{% highlight ruby %}
wsadmin.sh  -user  ****  -password  ****  -lang  jython  -c  "AdminApp.list()"
{% endhighlight %}
