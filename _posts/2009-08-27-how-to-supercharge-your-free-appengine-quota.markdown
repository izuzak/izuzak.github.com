---
layout: post
title: How to supercharge your free AppEngine quota?
---

_(this post was initially published on [my previous blog on wordpress.com](http://izuzak.wordpress.com/), you can visit it [here](http://izuzak.wordpress.com/2009/08/27/how-to-supercharge-your-free-appengine-quota/) and see the [comments](http://izuzak.wordpress.com/2009/08/27/how-to-supercharge-your-free-appengine-quota/#comments))_

<a href="http://code.google.com/appengine/" target="_blank">AppEngine</a> is <strong>awesome</strong>! Google's cloud computing platform is hosting countless web apps, from <a href="http://moderator.appspot.com/" target="_blank">Google</a> <a href="http://pubsubhubbub.appspot.com/" target="_blank">products</a> to innovative <a href="http://appgallery.appspot.com/" target="_blank">personal projects</a>, and is constantly being upgraded with cool <a href="http://code.google.com/appengine/docs/roadmap.html" target="_blank">new features</a>. <span style="background-color:#ffffff;">And best of all, AppEngine charges for resources (computing, storage and bandwidth) using a <a href="http://en.wikipedia.org/wiki/Freemium" target="_blank"><strong>freemium</strong></a><strong> </strong>pricing model:</span>

<img class="aligncenter" title="Google AppEngine" src="http://code.google.com/appengine/images/appengine_lowres.gif" alt="" width="115" height="88" /><em>"Each App Engine application can consume a certain level of computing resources for free, controlled by a set of </em><a style="color:#0000cc;" href="http://code.google.com/appengine/docs/quotas.html"><em>quotas</em></a><em>. Developers who want to grow their applications beyond these </em><strong><em>free quotas</em></strong><em> can do so by enabling </em><strong><em>billing </em></strong><em>for their application and set a daily resource budget, which will allow for the purchasing of additional resources if and when they are needed."</em>

For example, the default free quota has a daily limit of 6.5 hours of CPU time with a maximum rate of 15 CPU-minutes/minute, while the billing enabled quota has 6.5 hours of free CPU time plus a maximum of 1,729 hours of paid CPU time with a maximum rate of 72 CPU-minutes/minute.

<a href="http://code.google.com/appengine/docs/billing.html" target="_blank">Billing</a> for paid resources is controlled with a <strong>daily budget</strong> which developers set for each of their applications. The daily budget controls the amount of extra resources which may be purchased each day if the app steps over the free quota. The application developer defines how the daily budget is split between each of the <strong>billable quotas</strong> - outgoing bandwidth, incoming bandwidth, CPU time, stored data and number of recipients emailed. For example, <strong>$1 </strong>(the minimum daily budget) gets you 5 more CPU hours ($0.10 per CPU hour) and <strong>4 more GB of outgoing bandwidth </strong>($0.12 per GB).

Other than billable quotas, there are also <strong>unbillable quotas</strong> which increase when you switch to the paid model but are not billed. So, <strong>by switching to the paid model, irregardless to how you distribute your budget, your app gets a free</strong><strong> bump</strong><em> </em>in the:
<ul>
	<li>number of requests it can process from <strong>1,300,000</strong> to <strong>43,000,000</strong> and from <strong>7,400</strong> to <strong>30,000</strong> req/min,</li>
	<li>number of outgoing HTTP requests from <strong>657,000</strong> to <strong>46,000,000</strong> and from <strong>3,000</strong> to <strong>32,000</strong> req/min,</li>
	<li>number of memcache calls from <strong>8,600,000 </strong>to <strong>96,000,000</strong> and from <strong>48,000</strong> to <strong>108,000</strong> calls/min,</li>
</ul>
and many others. These are very usable improvements by themselves for apps that need to process a lot of requests (or bursts of requests) which don't consume a lot of CPU time, or apps that need to make a lot of outgoing HTTP requests that don't consume a lot of bandwidth.

So here's the idea for <strong>supercharging your free AppEngine quotas</strong>:
<ol>
	<li>switch you app to the paid model by enabling billing,</li>
	<li>enter the minimum daily budget ($1),</li>
	<li>distribute the budget over resources you are 100% sure will not consume their free quota (e.g. if you have a stateless app which doesn't use the database, put the whole $1 in the stored data quota).</li>
</ol>
Since you put your budget on resources which won't consume the entire free quota and since AppEngine doesn't charge you anything if the app doesn't step over the free quota - you are essentially getting a <strong>better free quota</strong>.

<em><strong>EDIT (Sept 5 2009):</strong></em> Yesterday, a week after my original post, Google released AppEngine SDK 1.2.5 which includes a major feature a lot of developers have been waiting for - <strong>XMPP support</strong>. What's interesting and relevant for this post is that XMPP has <a href="http://code.google.com/appengine/docs/quotas.html#XMPP">it's own quotas</a> for the number of API calls, data sent, recipients messaged and invitations sent, and - <strong>none of these quotas are marked as billable</strong>. Therefore, using the recipe for supercharging your free quota will also get you a significant boost in your XMPP quotas (657k -&gt; 46000k API calls, 657k -&gt; 46000k recipients messaged and 1k -&gt; 100k invitations sent). Pretty cool!

<em><strong>EDIT (Nov 2 2009):</strong></em> I just noticed that it is <strong>possible to increase the limit on the number of apps each AppEngine user can create</strong>. Currently, each user may create up to 10 AppEngine apps and there was no way to create more when you reached the limit, even by paying (or was there?). However, Google engineers have been approving explicit user requests for increases in the number of apps created <a href="http://groups.google.com/group/google-appengine/browse_thread/thread/210ebae3dc88cc6a?hl=en">on the official AppEngine group</a>. People who have requested an increase have had the limit set to 20 apps.

Are there any other hacks for squeezing more out of AppEngine?  <a href="http://twitter.com/izuzak" target="_blank">@izuzak</a>
