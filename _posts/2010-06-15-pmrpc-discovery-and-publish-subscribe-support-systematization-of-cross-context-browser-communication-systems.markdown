---
layout: post
title: Pmrpc discovery and publish-subscribe support + systematization of cross-context browser communication systems
---

_(this post was initially published on [my previous blog on wordpress.com](http://izuzak.wordpress.com/), you can visit it [here](http://izuzak.wordpress.com/2010/06/15/pmrpc-discovery-and-publish-subscribe-support-systematization-of-cross-context-browser-communication-systems/) and see the [comments](http://izuzak.wordpress.com/2010/06/15/pmrpc-discovery-and-publish-subscribe-support-systematization-of-cross-context-browser-communication-systems/#comments))_


The <a href="http://code.google.com/p/pmrpc/" target="_blank">pmrpc</a> cross-context browser communication library has grown from the <a href="http://wp.me/poYaf-44" target="_blank">last time I blogged about it</a>. Last time I wrote about adding support for RPC-style communication for <a href="http://www.whatwg.org/specs/web-workers/current-work/" target="_blank">WebWorkers</a>. Today I'll first introduce two new features - dynamic <strong>discovery of remote procedures</strong> and a<strong> publish-subscribe communication model</strong>. Also, I'll write about our <a href="http://code.google.com/p/pmrpc/wiki/IWCProjects" target="_blank"><strong>systematization of existing systems for cross-context browser communication</strong></a>.
<h2>Discovery and publish subscribe</h2>
Up until now, pmrpc supported only <a href="http://en.wikipedia.org/wiki/Remote_procedure_call" target="_blank">remote procedure call</a> as the communication model for inter-window and WebWorker communication. This model of communication is based on a client-server interaction where the client both a) has an object reference of the destination window/webworker context and b) knows the name of the remote procedure it wants to call. Here's an example of a pmrpc RPC call:

{% highlight javascript linenos %}
// server [window object A] exposes a procedure in it's window
pmrpc.register( {
  publicProcedureName : "HelloPMRPC",
  procedure : function(printParam) { alert(printParam); }
} );
{% endhighlight %}

{% highlight javascript linenos %}
// client [window object B] calls the exposed procedure remotely, from its own window
pmrpc.call( {
  destination : window.frames["myIframe"],
  publicProcedureName : "HelloPMRPC",
  params : ["Hello World!"]
} );
{% endhighlight %}

This works well for cases when the client knows how to obtain a reference of the server context object. However, it turns out that in many <a href="http://groups.google.com/group/pmrpc/browse_thread/thread/1a63a34971fd816e#" target="_blank">real-world</a> <a href="http://lists.whatwg.org/pipermail/whatwg-whatwg.org/2009-September/022940.html" target="_blank">cases</a> <strong>the client </strong><strong>does not know which window/iframe/worker implements the procedure it wants to call</strong>. Mashups and widget portals are the best example. Imagine that both the client and the server are widgets on <a href="http://www.google.com/ig" target="_blank">iGoogle</a>. Although you may be in control of both the client and server iframe contents, in most cases you will not know the names of iframe elements which contain them and which are located on the top container page. And you need the values of the name attributes so that you can obtain a reference to the iframe object which you could then use to call postMessage.

This referencing problem was the motivation for implementing both the discovery and publish-subscribe features. Both features enable clients to call a remote procedure without a-priori knowing which destination object implements the  procedure. First, the<strong> publish-subscribe feature</strong> enables clients to perform a remote call without specifying the destination context object reference. If a publish-subscribe call is made, the pmrpc infrastructure will make a RPC call for the specified procedure to all window, iframe and WebWorker object it can find (thus simulating a "publish" event on a channel named by the remote procedure name). The API for using the publish-subscribe feature is simple; it reuses the <em>pmrpc.call</em> method and instead of passing the reference of the destination context object, a "publish" string parameter is passed instead:

{% highlight javascript linenos %}
// server [window object A] exposes a procedure in it's window
pmrpc.register( {
  publicProcedureName : "HelloPMRPC",
  procedure : function(printParam) { alert(printParam); }
} );
{% endhighlight %}

{% highlight javascript linenos %}
// client [window object B] calls the exposed procedure remotely, from its own window
pmrpc.call( {
  destination : "publish",
  publicProcedureName : "HelloPMRPC",
  params : ["Hello World!"]
} );
{% endhighlight %}

Under the hood, the publish-subscribe feature uses the discovery feature to discover all window, iframe and WebWorker context objects and retrieves their references. Also, the publish-subscribe feature may be used with <a href="http://code.google.com/p/pmrpc/wiki/PmrpcApiDocs" target="_blank">other pmrpc features</a>, like access control and retries.

The <strong>discovery feature</strong> enables dynamic discovery of procedures registered at remote contexts and their filtering using various optional criteria. Discovered procedures may be filtered by the destination window/iframe/webworker context object, destination context origin (specified as a regular expression) and destination procedure name (also specified as a regular expression). For each discovered procedure that wasn't filtered out, the procedure name, origin of the destination context, the access control rules specified for the procedure and the destination context object. The feature is implemented as a <strong>new pmrpc method</strong> - <em>pmrpc.discover</em>. The method is asynchronous and accepts a parameter object which specifies the filters and a callback method which will be called when all registered procedures are discovered. Here's an example:

{% highlight javascript linenos %}
// server [window object A] exposes a procedure in it's window
pmrpc.register( {
  publicProcedureName : "HelloPMRPC",
  procedure : function(printParam) { alert(printParam); }
} );
{% endhighlight %}

{% highlight javascript linenos %}
// client [window object B] discovers and calls the exposed procedure remotely, from its own window
var discoCallback = function(discoveredProcedures) {
  if (discoveredProcedures.length > 0) {
    console.log(discoveredProcedures[0].publicProcedureName);
    console.log(discoveredProcedures[0].destinationOrigin);
    console.log(discoveredProcedures[0].procedureACL);
    pmrpc.call( {
      destination : discoveredProcedures[0].destination,
      publicProcedureName : "HelloPMRPC",
      params : ["Hello World!"]
    } );
  }
};
pmrpc.discover( {
  destination : [window.frames["someiFrameName"], window.frames["myIframe"]],
  origin : ".*goodOrigin.*",
  publicProcedureName : ".*Hello.*",
  callback : discoCallback
} );
{% endhighlight %}

The implementation of the discover method is based on <strong>recursively visiting every iframe in the window.frames object starting from window.top</strong>:
<ol>
	<li>The method fetches a reference to the top window object (window.top), adds the reference to the result array and repeats the procedure for each nested iframe (by iterating through the window.frames object). This way, all iframes in the current window are visited. This process is depicted by the figure below.</li>
	<li>The method adds all local web worker objects to the result array.</li>
	<li>On each context object in the result array, a special pmrpc infrastructure method is invoked to obtain a list of registered procedures.</li>
	<li>The list of all obtained procedures is filtered by the specified criteria.</li>
	<li>The method invokes the callback, passing the list of discovered procedures as a parameter.</li>
</ol>
It's not an ideal way of discovering remote contexts, but it's better than nothing and works with the latest versions of all major browsers.

<img class="aligncenter" title="Discovery architecture" src="http://izuzak.files.wordpress.com/2010/06/discoarchitecture-1.png" alt="" width="100%" />

<h2>Systematization of cross-context browser communication systems</h2>
It's evident that the WWW is becoming ever more componentized as almost every web site is a mashup, contains widgets or uses WebWorkers. As a result, there has been a increase in development of systems that enable communication between these different browser contexts - windows, iframes and WebWorkers. Pmrpc is only one example of such system and lots of other systems exist. However, as the number of these systems increases, the<strong> understanding of each systems' capabilities and their comparison and evaluation becomes a more difficult task</strong> since no effort is being made to systematize this ecosystem.

This is why <a href="http://twitter.com/ivankovic_42" target="_blank">Marko</a> and I are trying to create a <strong>systematization of existing cross-context communication systems</strong>. Our work on this systematization is in its early stages but is openly available to anyone at a <a href="http://code.google.com/p/pmrpc/wiki/IWCProjects" target="_blank">pmrpc wiki page</a>. At the moment we are trying to do the following:
<ol>
	<li>Make a list of all mechanisms for cross-context communication. We are including both in-browser mechanisms like the postMessage API, cookies and URL fragment, and 3rd party libraries like <a href="http://easyxdm.net/" target="_blank">easyXDM</a> and pmrpc.</li>
	<li>Define <strong>dimensions for classifying the gathered systems</strong>. Each dimension defines a system characteristic whereas values along each dimension define a set of possible design choices.  Currently, we have defined about a dozen dimension and are still thinking some through. Here are some examples: <em>Type of system</em> (browser built-in, pure client-side framework, server-mediated initialization framework, server-mediated communication framework), <em>Transport mechanism</em> (cookies, URL fragment, postMessage, ...), <em>Programming model</em> (message-oriented, shared memory, RPC, publish-subscribe).</li>
	<li>Evaluate the listed systems according to the defined dimensions.</li>
</ol>
I'd <a href="http://groups.google.com/group/pmrpc" target="_blank">love to hear</a> what you think of the new pmrpc features and the systematization (see more examples at our <a href="http://code.google.com/p/pmrpc/wiki/PmrpcApiDocs" target="_blank">API docs page</a>)! Do you know of any cross-context communication systems that are not listed? Do you think some other system characteristic should be included in the classification? <a href="http://twitter.com/izuzak" target="_blank">@izuzak</a>
