---
layout: post
title: "The Web engineer's online toolbox"
tags: web, engineering, software, development, testing, programming, http, toolbox, tools, online
commentIssueId: 15
---

I wanted to compile a list of online, Web-based tools that Web engineers can use for their work in development, testing, debugging and documentation. 
The requirements for a tool to make the list are: 

* must be a live Web application (no extensions or apps you have to host yourself), 
* free to use (some kind of free plan available),
* generic applicability (not usable only for a specific application/platform), 
* and must be useful to Web engineers (not just for Web site design). 

The current version of the list is shown below and is based on tools which I use or have used.
Two of my personal favorites are [RequestBin](http://requestb.in/) and [Hurl](http://hurl.it) which I use regularly for almost every project.
A few of the tools I haven't used in real projects but just played with them and thought they were really cool.

Since I've probably forgotten some excellent tools, and because new tools will be made in the future - the list will be a "live list", updated as the set of available tools changes. 
I didn't categorize the tools in the list since many tools may be used in many phases of software development.

Want to suggest a new tool or help me write better short descriptions of the tools? Add a comment below or make a [pull request](https://github.com/izuzak/izuzak.github.com).

**The Web engineer's online toolbox**

* **[RequestBin](http://requestb.in/)**
  * Lets you create a URL that will collect requests made to it, then let you inspect them in a human-friendly way.
* **[Hurl](http://hurl.it)**
  * Makes HTTP requests. Enter a URL, set some headers, view the response, then share it with others.
  * Similar tools: [REST test test](http://resttesttest.com/), [Apigee console](https://apigee.com/console/others).
* **[httpbin](http://httpbin.org/)**
  * A HTTP request & response service that covers all kinds of HTTP scenarios (e.g. different HTTP verbs, status codes and redirects).  
  * Similar tools: [UrlEcho](http://ivanzuzak.info/urlecho/).
* **[REDbot](http://redbot.org/)**
  * A robot that checks HTTP resources to see how they'll behave, pointing out common problems and suggesting improvements.
  * Similar tools: [HTTP lint](http://zamez.org/httplint).
* **[WebGun](http://webgun.io/)**
  * API for creating templated webhooks.
  * Similar tools: [UrlReq](https://github.com/izuzak/urlreq).
* **[Apify](http://apify.heroku.com)**
  * Exposes datasets locked within HTML documents for which there are no APIs. APIfy extracts data from structured markups and converts it to JSON APIs.
* **[Unicorn](http://validator.w3.org/unicorn/)**
  * W3C's unified validator, which performs a variety of checks from the popular HTML and CSS validators, as well as other useful services.
  * Similar tools: [HTML lint](http://lint.brihten.com/html/).
* **[Feed validator](http://validator.w3.org/feed/)**
  * W3C's validator for RSS and ATOM syndicated feeds.
* **[Link checker](http://validator.w3.org/checklink)**
  * Extracts links (recursively) from a Web site and checks that no link is defined twice, that all the links are dereferenceable and warns about HTTP redirects.
* **[Host tracker](http://www.host-tracker.com/)**
  * Website monitoring service with distributed ping/trace check, periodic monitoring, email/sms/IM notifications and statistics.
  * Similar tools: [Down for everyone or just me](http://www.downforeveryoneorjustme.com/), [Pimgdom ping service](http://tools.pingdom.com/ping/).
* **[Pingdom Full page test](http://tools.pingdom.com/fpt/)**
  * Enables users to test the load time of a page, analyze it, monitor, find bottlenecks and export results in HAR format.
  * Similar tools: [Web page test](http://www.webpagetest.org/).
* **[HAR viewer](http://www.softwareishard.com/har/viewer/)**
  * Visualizes HTTP Archive (HAR) log files created by HTTP tracking tools. 
* **[CORS proxy](http://www.corsproxy.com/)**
  * Allows JavaScript code on your site to access resources on other domains that would normally be blocked due to the same-origin policy.
* **[Browserling](https://browserling.com/)**
  * Interactive cross-browser testing using all major browsers and versions. Browsers within your browser.
* **[WebSocket Echo Test](http://www.websocket.org/echo.html)**
  * Test a WebSocket connection from your browser against our WebSocket echo server.
* **[YQL](http://developer.yahoo.com/yql/)**
  * An expressive SQL-like language that lets you query, filter, and join data across Web services
* **[Yahoo Pipes](http://pipes.yahoo.com/pipes/)**
  * A graphical user interface for building data mashups that aggregate web feeds, web pages, and other services.
* **[Apiary](http://apiary.io/)**
  * Language and tool for generating REST API documentation with an interactive inspector.
  * Similar tools: [Swagger](http://swagger.wordnik.com/).