---
layout: post
title: RPC for Web Workers and distributed computing within the browser
---

_(this post was initially published on [my previous blog on wordpress.com](http://izuzak.wordpress.com/), you can visit it [here](http://izuzak.wordpress.com/2009/12/21/rpc-for-web-workers-and-distributed-computing-within-the-browser/) and see the [comments](http://izuzak.wordpress.com/2009/12/21/rpc-for-web-workers-and-distributed-computing-within-the-browser/#comments))_

<a href="http://izuzak.wordpress.com/2009/10/10/inter-window-browser-communication-and-how-to-make-it-better/" target="_blank">In a previous post</a> I wrote about <a href="http://code.google.com/p/pmrpc" target="_blank">pmrpc</a> - a project of mine for developing a JavaScript library for <strong>cross-domain inter-window RPC-style communication in HTML5 browsers</strong>. RPC communication is based on two technologies: <a href="http://dev.w3.org/html5/spec/Overview.html#crossDocumentMessages" target="_blank">HTML5 cross-domain messaging</a> as the transport mechanism and <a href="http://groups.google.com/group/json-rpc/web/json-rpc-1-2-proposal" target="_blank">JSON-RPC</a> as the RPC protocol and message format, while the library supports other advanced features (access control, asynchronous calls, retries...). At the end of the post I outlined a few things <a href="http://twitter.com/ivankovic_42" target="_blank">Marko</a> and I wanted to implement next in pmrpc, one of which was support for Web Workers. From the post:
<blockquote><a href="http://www.whatwg.org/specs/web-workers/current-work/" target="_blank"><strong>Web Workers</strong></a> are another specification being developed in parallel with HTML5. The specification “defines an API for running scripts in the background independently of any user interface scripts”. What’s interesting is that the postMessage API is used to communicate with web workers and therefor pmrpc could be extended to include support for web workers.</blockquote>
Well, the API used to send messages from a web page to a worker and vice versa is not exactly the same as the cross-document messaging API, but is very similar. So, the idea for supporting Web Workers was leave the <a href="http://code.google.com/p/pmrpc/wiki/PmrpcApiDocs" target="_blank">pmrpc API</a> for registering, calling and unregistering procedure unchanged, and just extend it to also use the Web Worker API for message transport (in addition to the cross-doc messaging). And about two weeks ago we released <a href="http://code.google.com/p/pmrpc/downloads/list" target="_blank">a new version of pmrpc</a> with Web Worker support. Here's an example of <strong>using pmrpc with Web Workers</strong>:

{% highlight javascript linenos %}
// [worker object A - testingWorker.js]

// load pmrpc library
importScripts('pmrpc.js');

// expose a procedure
pmrpc.register( {
publicProcedureName : "HelloPMRPC",
procedure : function(printParam) {
              alert(printParam); } } );
{% endhighlight %}

{% highlight javascript linenos %}

// [window object B]

// load pmrpc library

// create worker
var testingWorker =
  new Worker("testingWorker.js");

// calls the exposed procedure
pmrpc.call( {
destination : testingWorker,
publicProcedureName : "HelloPMRPC",
params : ["Hello World!"] } );
{% endhighlight %}

A few notes on worker support. Pmrpc currently supports only normal ("dedicated") Web Workers. Shared Workers (workers shared between multiple web pages) and nested workers (workers within workers) are currently not supported since, as far as I know,<strong> Shared and nested Workers are not implemented yet in any browser</strong>. Also, since Workers must be loaded from the same origin as the page that created them - the <a href="http://code.google.com/p/pmrpc/wiki/PmrpcApiDocs#Access_control" target="_blank">access control feature</a> in pmrpc make no sense and are therefore not supported for Web Workers.

Next on our list is enabling <strong>discovery of registered procedures</strong> scattered over multiple browser workers, iframes and windows. Currently, pmrpc requires that you tell it where the procedure you are calling is, but sometimes you don't know where it is, or don't care - e.g. you just want to call the "DrawMap" procedure loaded from domain "http://www.good.com" if it exists. Discovery should enable discovery of methods based just on their names. The problems in implementing this feature (knowing which windows, iframes and workers are currently running and syncing the list of registered procedures with each of them) made me think about where systems running in browsers are heading - towards a form of <strong>distributed &amp; parallel computing, only within a single browser</strong>. Distributed and concurrent execution, discovery, access control, synchronization, remote invocation, fault tolerance and naming are all concepts well know in distributed computing, and will possibly be re-invented in the context of web browsers to support this new shift. I'm not sure how far pmrpc will go in this direction, but I hope it will be a good example for other developers and research projects.

Do you think that web browsers need more powerful mechanisms supporting this new form of distributed computing? <a href="http://www.twitter.com/izuzak" target="_blank">@izuzak</a>

