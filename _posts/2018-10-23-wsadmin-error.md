---
layout: post
title:  "I get an error : wsadmin.sh could not connect to the selected profile when I run the data collector"
date:   2018-10-22
categories: Datacollector Execution
---

For wsadmin.sh to work the WAS system has to be running. Please check that it is. Also check that you are using the correct credentials. You can check your credentials using this command to see if it connects : 

{% highlight ruby %}
wsadmin.sh  -user  ****  -password  ****  -lang  jython  -c  "AdminApp.list()"
{% endhighlight %}
