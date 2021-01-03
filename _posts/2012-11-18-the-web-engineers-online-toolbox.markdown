---
layout: post
title: The Web engineer's online toolbox
description: The Web engineer's online toolbox
tags: web, engineering, software, development, testing, programming, http, toolbox, tools, online
commentIssueId: 16
---

I wanted to compile a list of online, Web-based tools that Web engineers can use for their work in development, testing, debugging and documentation.
The requirements for a tool to make the list are:

* must be a live Web application (no extensions or apps you have to host yourself),
* free to use (some kind of free plan available),
* generic applicability (not usable only for a specific application/platform),
* and must be useful to Web engineers (not just for Web site design)

The current version of the list is shown below and is based on tools which I use or have used.
Two of my personal favorites are [RequestBin](http://requestbin.com/) and Hurl (https://github.com/defunkt/hurl) which I use regularly for almost every project.
A few of the tools I haven't used in real projects but just played with them and thought they were really cool.

Since I've probably forgotten some excellent tools, and because new tools will be made in the future - the list will be a "live list", updated as the set of available tools changes.
I didn't categorize the tools in the list since many tools may be used in many phases of software development.

Want to suggest a new tool or help me write better short descriptions of the tools? Add a comment below or make a [pull request](https://github.com/izuzak/izuzak.github.com).

**The Web engineer's online toolbox** *(last edited on July 21 2013)*

* **[RequestBin](http://requestbin.com/)**
  * Lets you create a URL that will collect requests made to it, then let you inspect them in a human-friendly way.
  * Note: the original hosted version of RequestBin, developed by Runscope, was taken down in 2018. The code repository remains online   at  [https://github.com/Runscope/requestbin](https://github.com/Runscope/requestbin).
* **[Hoppscotch](https://hoppscotch.io/)**
  * Makes HTTP requests. Enter a URL, set some headers, view the response.
  * Similar tools: [REST test test](http://resttesttest.com/), [Apigee console](https://apigee.com/console/others), [Web-Sniffer](http://web-sniffer.net/).
* **[Runscope](https://www.runscope.com/)**
  * A proxy for inspecting API calls, providing several features of Hurl and RequestBin.
* **[httpbin](http://httpbin.org/)**
  * A HTTP request & response service that covers all kinds of HTTP scenarios (e.g. different HTTP verbs, status codes and redirects).
  * Similar tools: [httpstat.us](http://httpstat.us/), [Mock Response](http://mock.isssues.com/), [Hang](http://hang.nodester.com/), [UrlEcho](http://ivanzuzak.info/urlecho/), [Mocky](http://www.mocky.io/).
* **[REDbot](http://redbot.org/)**
  * A robot that checks HTTP resources to see how they'll behave, pointing out common problems and suggesting improvements.
  * Similar tools: [HTTP lint](http://zamez.org/httplint).
* **[WebGun](http://webgun.io/)**
  * API for creating templated webhooks.
  * Similar tools: [UrlReq](https://github.com/izuzak/urlreq).
* **[Webscript](https://www.webscript.io/)**
  * Webscripts are short and hosted scripts, written in Lua. They can respond to HTTP requests or run as cron jobs.
* **[ClickHooks](http://www.clickhooks.com/)**
  * A URL shortener service that provides callbacks via HTTP POST when people go to a shortened link.
* **[MailHooks](http://mailhooks2.appspot.com/)**
  * Lets you receive emails via HTTP POST (aka webhooks). You can create many hooks that will give you an e-mail address that, when e-mailed, parses and POSTs the message to the URL you specify.
* **[Quilla](http://a.quil.la/)**
  * Provides short-links where people can contact you, where each submission on your short-link will send you an email. A kind of a HTTP->SMTP proxy.
* **[Apify](http://apify.heroku.com)**
  * Exposes datasets locked within HTML documents for which there are no APIs. APIfy extracts data from structured markups and converts it to JSON APIs.
* **[Unicorn](http://validator.w3.org/unicorn/)**
  * W3C's unified validator, which performs a variety of checks from the popular HTML and CSS validators, as well as other useful services.
  * Similar tools: [HTML lint](http://lint.brihten.com/html/).
* **[HTML tidy](http://infohound.net/tidy/)**
  * HTML Tidy is a tool for checking and cleaning up HTML source files.
* **[Feed validator](http://validator.w3.org/feed/)**
  * W3C's validator for RSS and ATOM syndicated feeds.
* **[JSONLint](http://jsonlint.com/)**
  * JSON validator.
* **[Link checker](http://validator.w3.org/checklink)**
  * Extracts links (recursively) from a Web site and checks that no link is defined twice, that all the links are dereferenceable and warns about HTTP redirects.
* **[Url Decode](http://urldecode.org/)** and **[Url Encode](http://urlencode.org/)
  * URL decode and encode strings.
* **[Host tracker](http://www.host-tracker.com/)**
  * Website monitoring service with distributed ping/trace check, periodic monitoring, email/sms/IM notifications and statistics.
  * Similar tools: [Down for everyone or just me](http://www.downforeveryoneorjustme.com/), [Pingdom ping service](http://tools.pingdom.com/ping/), [IsItUp](http://isitup.org/), [Ping brigade](https://www.pingbrigade.com/).
* **[ViewDNS](http://www.viewdns.info/)**
  * Collection of DNS and network-level tools, such as reverse IP lookup, DNS record lookup and traceroute.
* **[Necrohost](http://www.necrohost.com/)**
  * List of URLs that simulate a variety of network connectivity issues e.g. slow response, unresolvable DNS and 404.
* **[Mirrorrr](https://code.google.com/p/mirrorrr/)**
  *  Application that mirrors the content of URLs you supply. Rewrites the fetched page to mirror all content, including images, Flash, Javascript, CSS, and even favicons.
* **[SSL Checker](http://certlogik.com/ssl-checker/)**
  * Test SSL certificates of online sites and helps with identifying any problems.
* **[CSR/Cert decoder](http://certlogik.com/decoder/)**
  * Decode and check your CSRs and SSL certificates.
* **[SSL Server Test](https://www.ssllabs.com/ssltest/index.html)**
  * Performs a deep analysis of the configuration of any SSL web server on the public Internet.
* **[Loadzen](http://loadzen.com/)**
  * Cloud-powered load testing service.
* **[Pingdom Full page test](http://tools.pingdom.com/fpt/)**
  * Enables users to test the load time of a page, analyze it, monitor, find bottlenecks and export results in HAR format.
  * Similar tools: [Web page test](http://www.webpagetest.org/).
* **[Google PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights)**
  * Analyzes the content of a web page, then generates suggestions to make that page faster.
* **[Dotcom-Tools Website Speed Test](https://www.dotcom-tools.com/website-speed-test.aspx)**
  * Test your website speed from 25 locations worldwide.
* **[HAR viewer](http://www.softwareishard.com/har/viewer/)**
  * Visualizes HTTP Archive (HAR) log files created by HTTP tracking tools.
* **[PCAP Web Performance Analyzer](http://pcapperf.appspot.com/)**
  * Converts a PCAP file to HAR, provides a HTTP waterfall view using HarViewer and Page Speed suggestions for your network trace.
* **[CORS proxy](http://www.corsproxy.com/)**
  * Allows JavaScript code on your site to access resources on other domains that would normally be blocked due to the same-origin policy.
* **[Browserling](https://browserling.com/)**
  * Interactive cross-browser testing using all major browsers and versions. Browsers within your browser.
* **[WebSocket Echo Test](http://www.websocket.org/echo.html)**
  * Test a WebSocket connection from your browser against our WebSocket echo server.
* **[YQL](http://developer.yahoo.com/yql/)**
  * An expressive SQL-like language that lets you query, filter, and join data across Web services.
* **[Webshell](http://webshell.io/)**
  * An API which allow accessing any data on the programmable Web using APIs.
* **[Yahoo Pipes](http://pipes.yahoo.com/pipes/)**
  * A graphical user interface for building data mashups that aggregate web feeds, web pages, and other services.
* **[Apiary](http://apiary.io/)**
  * Language and tool for generating REST API documentation with an interactive inspector.
  * Similar tools: [Swagger](http://swagger.wordnik.com/).
* **[JSFiddle](http://jsfiddle.net/)**
  * A playground for Web site developers: code editor and repository for snippets built from HTML, CSS and JavaScript.
  * Similar tools: [JSBin](http://jsbin.com/)
* **[Google Feed API](https://developers.google.com/feed/v1/jsondevguide)**
  * Enables looking up feed URLs for a site ([example](http://ajax.googleapis.com/ajax/services/feed/lookup?v=1.0&q=http://ivanzuzak.info/)), searching for feeds by keywords ([example](https://ajax.googleapis.com/ajax/services/feed/find?v=1.0&q=ivan%20zuzak)) and returning a JSON representation of a feed ([example](https://ajax.googleapis.com/ajax/services/feed/load?v=1.0&q=http://ivanzuzak.info/atom.xml)).
* **[About My Browser](https://aboutmybrowser.com/?nr)**
  * Extracts and displays browser info (browser, version, OS, cookies, JS, screen resolution, etc) and stores this info as a shareable Web page.
  * Similar tools: [http://browserspy.dk/](http://browserspy.dk/), [SupportDetails](http://supportdetails.com/)
* **[ExtendsClass](https://extendsclass.com/)**
  * Provides random data generators (JSON, CSV, SQL), database playgrounds (MySQL, SQLite, PostgreSQL, SQL Server), code testers (PHP, Java), API tools (REST/SOAP clients, Mock, JSON storage), ...
