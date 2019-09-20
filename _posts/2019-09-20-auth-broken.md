---
layout: post
title: I get an "Authentication endpoint is broken" message when I try to access the TA UI.
categories: General
author: Kieran
---

There is a known issue on Transformation Advisor 2.0.1 when authentication tokens expire. To resolve please delete the `ta-access-token-cookie` cookie in your browser and refresh. Alternatively you can quit and restart your browser which will delete the cookie automatically.

 
