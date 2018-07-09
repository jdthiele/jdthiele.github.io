---
layout: post
title:  "Automated Weblogic Server-Start Changes"
date:   2018-07-09 10:34:00 -0500
categories: python xml script weblogic wls automation NewRelic monitoring APM
---

### Download the script
Download from Github: <https://github.com/jdthiele/weblogic-server-start-changes>

### Overview
I hope you like my new domain name. :-)

I am working on a project to implement a Technology Operations Center (TOC) which is basically a Network Operations Center but with a wider 
scope to also include application and services stack that IT provides to the organization. We are working to improve the visibility into the 
health and operations of the critical IT systems and their dependent components. It is a challenging and complex project but it has been rewarding so far. 

One of the monitoring tools that we focused on out of the gates was New Relic. New Relic is a great application performance monitoring tool 
(among other functionality) that has deep visibility into custom developed applications. We are using New Relic to monitor the custom ERP software that,
like most ERP software installs, is the most centralized and critical pieces of business software an organization has. There are tons of dependencies
within the software itself, but then also down into the infrstructure that supports it. We are using other monitoring tools, like SolarWinds,  already 
for that infrastructure layer, but New Relic fills the gap that we had in the application layer.

We have gone through several different iterations of the configuration and naming of the different application modules due to the complexity of the 
architecture of the custom ERP software we are monitoring. We discovered that we were missing some of the granularity we need in our metrics
because we have the applications grouped at too high of a level. We needed to group the applications a layer lower, basically down at the managed server/JVM
level instead of the host level. In order to make this change, we need to touch the WebLogic configuration for 250 managed servers in just the UAT environment,
as noted in the documentation available in the WebLogic section here: <https://docs.newrelic.com/docs/agents/java-agent/installation/include-java-agent-jvm-argument>
The instructions provided require logging into the WebLogic console of all 250 managed servers and entering a unique argument 
into the "server-start" field. That argument is also unique for each managed server, making it very error prone. We really needed a way to automate it.
New Relic did not have an option for us and Googling provided no clear options either. So we decided to try to automate it ourselves. We first found where
the configurations were stored in the file system: `${WLS_HOME}/user_projects/domains/base_domain/config/config.xml`

After realizing I needed to parse XML, I quickly dug into the python docs to see how easy this would be, which turned out to be very easy! I had a 
spreadsheet that listed each host, managed server name, and the path we needed to configure for the "-javaagent" argument. All I needed to do was find if the
argument already existed, check it if so, and if not, add it to the list of any existing server-start arguments. This was my first time parsing XML with Python.
It was easier than I expected, probably due to the great help available online for Python developers, and those of us aspiring to become one.

Here's my script ([script.py](https://github.com/jdthiele/weblogic-server-start-changes)). Please do share your thoughts on the GitHub repo for now.

Thanks!
-D
