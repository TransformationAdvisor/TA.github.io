---
layout: post
title: Datacollector fails to generate server.xml artifacts
categories: Datacollector
author: Kieran Geoghegan
---
If the data collector is run as a user other than the user that has launched the deployment manager, the datacollector will fail to generate the server xml artifacts. 
The collected artifacts will look like a collection that has not requested configuration analysis. On TA versions earlier than 1.9.4, you may see the following error:

```
Exception in thread "main" java.io.FileNotFoundException: <some_path>_server.xml (A file or directory in the path name does not exist.)
        at java.io.FileInputStream.<init>(FileInputStream.java:113)
        at java.io.FileInputStream.<init>(FileInputStream.java:73)
		...
		...
```

You must run the data collector as the same user as has launched the deployment manager. You can check which user has launched the deployment manager with the ```ps``` command.

**Note:** If you have already unpacked the data collector as a different user, you may need to edit the permissions or owner for the unpacked data collector directory before you run the data collector as the new user. Alternatively you can simply unpack the data collector again as the new user.
