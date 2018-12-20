---
layout: post
title: Cusomized Binary Scanner Commands
categories: Datacollector
author: Niall Horgan
---

Customized commands for the binary scanner can be given using customCmd.properties file in the conf directory.

There is a different command for each report.

The value of java_opt will be used to customized JRE runtime for every binary scanner command.

By default all the commands are commented out.

Uncomment the command which you want to customize.

The DC use the complete command generating that specific report.

The DC will stop executing and throw an error if any of the options of custom command are incorrect.


DOS AND DON'TS WHILE WRITING THE CUSTOM COMMAND OPTIONS.
''''
1- Don't give --format option. This is because TA server expects both html and json formats for each report. Generating only one report format may introduce errors while uploading and presenting the results. 

2- Don't give --output option. This is because TA server expects file names in certain order. Moreover giving this option may change the location of reports storage which may cause problems while zipping up and uploading the results.

3- If an option needs a path as its value then always surround the paths with double quotes.
path="c:\\docs\\doc1"

4- The backslash character must be escaped as a double backslash. For example:
path="c:\\docs\\doc1"
''''
