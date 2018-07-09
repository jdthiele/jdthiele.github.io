---
layout: post
title:  "Python API into SolarWinds"
date:   2018-01-16 00:34:00 -0500
categories: python rest api orionsdk solarwinds script
---

### Download the script
Download the script here: <https://github.com/jdthiele/orionsdk-python_scripts>

### Overview
I finally did it! I finally wrote a useful python script/program! At least, I think it is useful... I have been dabbling with python here and there but never spent the time to actually make it work from start to finish to accomplish some goal or task. It seemed so daunting to learn because there is so much capability available to you. I am very comfortable writing bash scripts and hardly need to look at documentation anymore unless I am doing something a little more complex. Bash is the "language" that I think of when solving logic problems on Linux. I've known that I needed to add another more capable language to my repertoire, and I think with the completion of this first full script, I've taken my first steps into python-land. Yay! With those first steps, I now see the splendor that lies ahead. I'm ready to dive in.

This script was created as the first steps to learn and understand how I can begin working with the data behind SolarWinds. We use SolarWinds quite extensively at my current client for monitoring UNIX and Linux servers, networking gear, applications, tools and services. It's mostly infrastructure focused. There is a project underway that I am helping to lead where we are doing a complete review of the monitoring tools in place so that we can identify and implement solutions for any gaps and overlaps found. Part of the vision of this project is to end up with the ever elusive "single pane of glass" view point into our complete monitoring environment. Now, I can guarantee you that there is no tool out there that can do every bit of monitoring that an organization needs. There is also no tool or product that I have found yet that can connect to all of the existing tools and consolidate their data or views into their data. It's always been up to each organization to create their own solution to this problem, which ends up rarely ever leading to a true solution. All the silos end up using their own tools and seldom have the full visibility they need to troubleshoot and optimize. Their managers thereby also don't have the visibility they need to lead their teams towards higher levels of service for our end users.

I'm hoping that we can find a way to reach this elusive goal. I have some ideas for how this can be done and how it can be shared with the greater public that might find a solution like this useful. More to come on that later.

Back to this script. It's just a python script that connects into the [orionsdk](https://github.com/solarwinds/OrionSDK) API via the [orionsdk-python](https://github.com/solarwinds/orionsdk-python) project on GitHub that SolarWinds provides to its customers and runs a query to pull some data out from the Orion back end. There are obviously many capabilities built into the SDK other than querying data, but that was just a starting place for me. I will almost definitely spend more time figuring out how to create some reports, add new hosts in a bulk mode (not easy to do in the GUI), pull large amounts of data into a central monitoring repo, etc. As I dive deeping into those areas, I'll share more about them here.

Here's my script ([sw-query.py](https://github.com/jdthiele/orionsdk-python_scripts)). Please do share your thoughts on the GitHub repo for now.

Thanks!
-D
