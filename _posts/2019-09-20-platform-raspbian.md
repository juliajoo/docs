---
layout: post
title:  "Raspbian"
date:   2019-09-20 01:30:13 +0000
categories: Platform
tags:  RaspberryPi Raspbian
---

# Raspbian 

Raspbian is a linux based computer operating system for the Raspberry Pi. 


## Running Code on Startup 
A useful workflow for the pi, is to run some particular code always on startup.
A good way of doing that is by using the systemd interface.  You can set up this by 
**creating a service**, which starts with a service script.
Start by creating a service script in your Pi's terminal
(replace MY_EXAMPLE with an appropriate name):

<pre><code>
 sudo nano /etc/systemd/system/MY_EXAMPLE.service
</code></pre>


Now you can configure your service script (you can find more details [here](https://www.digitalocean.com/community/tutorials/understanding-systemd-units-and-unit-files)):
<pre><code>
[Unit]
Description=My description 
# After details if the service needs to wait for anything before
# running, you can put other "events" here
After=multi-user.target 
 
[Service]
Type=simple
# Note you must always give the absolute path, 
# You can get this by running the "pwd"
# in your command line in the file's location, 
# and appending the "/FILE_NAME" to the response
ExecStart=/usr/bin/python3 ABSOLUTE_PATH/YOUR_SCRIPT.py
Restart=always # if anything goes wrong, your service always restarts
 
[Install]
WantedBy=multi-user.target

</code></pre>

Now you can attempt to start the service with the command:
<pre><code>
# Start service
sudo systemctl start MY_EXAMPLE.service

</code></pre>

Use status to make sure the service started with no hiccups

<pre><code>
# Check status
sudo systemctl status MY_EXAMPLE.service
</code></pre>

Afterwards stop it, make sure it was stopped properly, and then
configure the service to start automatically at boot:

<pre><code>
# Stop service
sudo systemctl stop MY_EXAMPLE.service

# configure automatic start
sudo systemctl enable MY_EXAMPLE.service
</code></pre>


