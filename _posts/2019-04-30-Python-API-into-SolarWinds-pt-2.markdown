---
layout: post
title:  "Python API into SolarWinds - pt 2"
date:   2019-04-30 11:00:00 -0500
categories: python rest api orionsdk solarwinds script
---

### Download the script
Download the scripts here: <https://github.com/jdthiele/orionsdk-python_scripts>

### Overview
This is an update to the [original post]({{ site.baseurl}}{% post_url 2018-01-16-Python-API-into-SolarWinds %}).

I've added a couple of new scripts to the GitHub repo. The [sw-custom-query.py](https://github.com/jdthiele/orionsdk-python_scripts/blob/master/sw-custom-query.py)
script is basically an extension of the original sw-query.py that allows you to more easily build a collection of SWQL query files and call them via an argument 
in the script.

The [sw-update-machinetype.py](https://github.com/jdthiele/orionsdk-python_scripts/blob/master/sw-update-machinetype.py) script is something I created to greatly
improve the speed of changing our Linux hosts' MachineType fields from "net-snmp - Linux" to "Linux". Nice, clean, simple. Several years back, I came across this
[Thwack post](https://thwack.solarwinds.com/thread/61847) that explained a more laborious method of doing so, which involved customizing the snmpd.conf on all of
the servers to use a custom script that would pull into the OS type. Then I had to create a new poller that would reference that new OID and then go touch each server
inside SolarWinds to use that new poller for Node Details. However, after doing that each node would then change its name to its IP address. Ugh! Of course, that
means touching each node inside SolarWinds again to rename it back to its proper name. As a Linux command line guy, I thoroughly loath repetitious mouse clicking
for something that should be easy and automated. So that is what I did. This script uses the OrionSDK to update the necessary fields directly in the database without
having to touch each server multiple times or install the new Linux agent. The custom poller effectively did the same thing but required a lot more work to do it.

My scripts are available at ([GitHub](https://github.com/jdthiele/orionsdk-python_scripts)). Please do share your thoughts in the GitHub repo by submitting an issue
or pull request.

Thanks!
-D
