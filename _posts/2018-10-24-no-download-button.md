---
layout: post
title: “I performed a data collection. It was uploaded to my ICP. However I did not get download bundle button enabled. I am not able to download the bundle or server.xml files”
categories: Migration
author: “Niall Horgan“
---
You need to run the full analysis in order for Transformation Advisor to get the server configuration:

{% highlight ruby %}
bin\transformationadvisor.bat -w <WEBSPHERE_HOME_DIR> -p <PROFILE_NAME> <WSADMIN_USER> <WSADMIN_PASSWORD>
{% endhighlight %}
