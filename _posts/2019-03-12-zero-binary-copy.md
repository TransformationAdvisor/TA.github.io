---
layout: post
title: When I run the data collector against a specified profile, it skips the applications that were installed using zero binary copy mode. Why?
categories: Datacollector
author: Jean Gao
---
The reason we skip the applications that were installed with zero binary copy mode is because we are not able to get all of the required configurations for the applications to generate the migration recommendations.  It is user's own responsibility to evaluate the migration efforts for this type of applications.
