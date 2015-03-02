---
layout: recipe
title: Scrape like nobody is watching
excerpt: "Web scraping techniques straight out of the Langley headquarters."
categories: recipes
tags: [jmeter,testing,scraping]
image:
  thumb: recipes/2015-03-02-scrape-like-nobody-is-watching.png
  credit: http://www.audio-agent.com/wp-content/themes/audioAgent/images/agent.png
comments: true
share: false

ingredients: 
  - title: JMeter
    url: http://jmeter.apache.org/
    text: 1 instance of the Apache JMeter load testing tool
    
  - title: Tor
    url: https://www.torproject.org/
    text: 1 instance of the Tor service
   
  - title: Privoxy 
    url: http://www.privoxy.org
    text: 1 instance of the Privoxy service
---

## The problem

Ever got yourself in the situation where you '_had_' to scrape a web site for _'educational'_ reasons and found yourself in the unpleasant situation just when things started to get exciting, the web site gave you the nasty **401** response?  

Well, put on your agent ![alt text](http://cdn.flaticon.com/png/32/55854.png "test") and let's start cooking!

## The solution

### Install JMeter

Download the latest version of [JMeter](http://jmeter.apache.org/download_jmeter.cgi) and follow the instructions on how to install it locally. 

### Install Tor

>Tor is free software and an open network that helps you defend against traffic analysis, a form of network surveillance that threatens personal freedom and privacy, confidential business activities and relationships, and state security.

*Tor* is your passport to anonymity - we will use this service as a proxy for sending out our requests, without revealing our real I/P address.  Make sure you stay legit and keep it clean.   

Install the latest version of the Tor service (either standalone or with the browser) for your OS.

For Ubuntu type:

	$ sudo apt-get install tor


And that's pretty much it.  Ensure that the daemon is running by typing:

	$ sudo service tor status

### Install Privoxy

> Privoxy is a non-caching web proxy with advanced filtering capabilities for enhancing privacy, modifying web page data and HTTP headers, controlling access, and removing ads and other obnoxious Internet junk. Privoxy has a flexible configuration and can be customized to suit individual needs and tastes. It has application for both stand-alone systems and multi-user networks.

_Privoxy_ will act as a bridge between JMeter and Tor - the reason behind that is that Tor speaks the **SOCKS5** protocol, while JMeter doesn't.  Privoxy is here to help (as we usually don't want to start writing low-level code while using JMeter). 

Install the latest version of the Privoxy service for your OS.

For Ubuntu type:

	$ sudo apt-get install privoxy

We can make these  2 babies talk to each other by commenting out a single line in the config file.  Open Privoxy's config file

	$ sudo nano /etc/privoxy/config

and find the following line:

	# forward-socks5   /               127.0.0.1:9050 .

Now, un-comment this line.  What this will do, is to chain Privoxy and Tor services
running on the same system.  Privoxy will do the translation of the requests for us and forward them to Tor so that we don't have to do the nasty work.  Notice that the Tor service is listening on port **9050** by default and Privoxy on port **8118**.  If you've changed any of these ports, make sure you use the correct values.

### Restart the services

Now, that we have our communication setup it's time to restart the _agents_.  

	$ sudo service tor restart
	$ sudo service privoxy restart

### Configure JMeter to use the agents

Ok, now that we have our _secret agents_ up-n-running it's time to put them to work.  Go back to your test plan in JMeter and add an [HTTP Request Defaults config](http://jmeter.apache.org/usermanual/component_reference.html#HTTP_Request_Defaults) element at the beginning of the test plan.  Now, find the section titled *'Proxy Server'* and in the *'Server name or IP'* field enter **localhost** and in the *'Port number'* enter **8118**.

![](http://jmeter.apache.org/images/screenshots/http-config/http-request-defaults.png)
  
This will make all the HTTP requests sent from JMeter to go through the Privoxy daemon, which in turn will forward them to the Tor daemon, which in turn will forward them to the Tor network.  

### V for Vendetta

Well, that's pretty much it...!  Now every time you execute a test plan with the proxy settings on, Tor will delegate your request to one of the network's proxy servers and the server will _'hit'_ the target URL without revealing your IP address.

This way, you will never get your home's on company's IP blocked again.  

## Chef, I got blocked again... now what?

Well, in case the target site blocks you again - let's say due to too many parallel requests and a DOS filter - then bad luck :( you have to do something very difficult...

	$ sudo service tor restart

That's it - each time you restart the Tor service, a new delegate proxy server is allocated so you can carry on with the nasty thing :)

Bon Appétit 
