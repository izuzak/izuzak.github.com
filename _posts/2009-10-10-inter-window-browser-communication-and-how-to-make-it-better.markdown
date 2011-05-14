---
layout: post
title: Inter-window browser communication and how to make it better
commentIssueId: 5
---

As promised in my <a target="_blank" href="http://ivanzuzak.info/2009/08/09/named-arguments-in-javascript.html">post on named arguments in JavaScript</a>, this post will be about <a href="http://code.google.com/p/pmrpc">pmrpc</a> - a project I've been working on with <a target="_blank" href="http://mivankovic.blogspot.com/">Marko</a> for the last few months.

But first, a bit of background. From mid 2007. when I was interning at Google up until today and my dissertation-related research, an issue that's consistently been popping up in anything I worked on is inter-window communication. <strong>Inter-window communication (IWC)</strong> is the process of transporting data from one window object within a browser to another window object inside the same browser. What's relevant here is that window objects are both full-size windows and iframes - window objects that can be embedded within other window objects (think of an <a target="_blank" href="http://www.igoogle.com">iGoogle</a> page and all the gadgets in it).

<img src="http://code.google.com/gme/articles/images/mashupgadgets_igoogle.jpg" title="iGoogle" class="aligncenter" width="362" height="220">

The reason why IWC is an issue is the security model of communication within browsers. <strong><a target="_blank" href="http://en.wikipedia.org/wiki/Same_origin_policy">Same Origin Policy</a></strong> (SOP), a security policy introduced to reduce security risks, restricts access of processes executing within browsers. In essence, SOP prevents a document or script loaded from one domain from getting or setting properties of a document from another domain. For example, a page loaded from <em>http://www.google.com</em> can not directly access data in an iframe loaded from <em>http://www.yahoo.com</em>.

Although SOP turns Web applications into a set of isolated units, the <a target="_blank" href="http://groups.google.com/group/talk-about-widgets/web/use-cases-for-iwc">need for communication between those islands</a> is growing rapidly. With the advancement web technologies, web applications were slowly becoming componentized and many current web applications are based on <strong>mashing-up content from different sources</strong> - map gadgets, finance widgets, blog commenting toolboxes, games, chat widgets and <a target="_blank" href="http://www.google.com/ig/directory">many</a>, <a target="_blank" href="http://www.facebook.com/apps/directory.php">many</a> <a target="_blank" href="http://www.widgetbox.com/tag_cloud.jsp">others</a>. Since widgets, content encapsulated within an iframe, are the most popular method of including cross-domain content on a web page, <strong>inter-widget communication</strong> is most often implemented as inter-window communication.

A lot of <strong>groups and projects are researching models and developing systems</strong> that address the problem of communication between widgets and between windows in general. So here's a short overview of this broad space of R&amp;D. <a target="_blank" href="http://www.opensocial.org/Technical-Resources/opensocial-spec-v09/Gadgets-API-Specification.html">Various</a> <a target="_blank" href="http://www.w3.org/TR/widgets/">widget</a> <a target="_blank" href="http://www.openajax.org/member/wiki/OpenAjax_Metadata_1.0_Specification_Widget_Overview">specifications</a> <a target="_blank" href="http://dev.opera.com/articles/view/opera-widgets-specification-1-0-third-ed-2/">are trying</a> to formally define and standardize what a widget actually is. <a target="_blank" href="http://code.google.com/p/browsersec/w/list">Browser security in general</a> and <a target="_blank" href="http://w2spconf.com/2007/papers/paper-170-z_6423.pdf">various issues</a> concerning <a target="_blank" href="http://seclab.stanford.edu/websec/frames/post-message.pdf">intra-browser communication</a> and <a target="_blank" href="http://domino.research.ibm.com/library/cyberdig.nsf/papers/0EE2D79F8BE461CE8525731B0009404D/$File/RT0742.pdf">mashup security</a> are <a target="_blank" href="http://www.adambarth.com/papers/2009/barth-weinberger-song.pdf">being</a> <a target="_blank" href="http://www.adambarth.com/papers/2009/barth-jackson-li.pdf">heavily</a> <a target="_blank" href="http://www.adambarth.com/papers/2009/reis-barth-pizano.pdf">researched</a>. <a target="_blank" href="http://www.cs.rutgers.edu/~vinodg/papers/acsac2008b/acsac2008b.pdf">Low-level IWC mechanisms</a> are <a target="_blank" href="http://json.org/module.html">being</a> <a target="_blank" href="http://dev.w3.org/html5/spec/Overview.html#crossDocumentMessages">proposed</a> and <a target="_blank" href="http://code.google.com/p/xssinterface/">implemented</a>, sometimes even as <a target="_blank" href="http://tagneto.blogspot.com/2006/06/cross-domain-frame-communication-with.html">browser hacks</a>, and <a target="_blank" href="http://www.openajax.org/member/wiki/OpenAjax_Hub_1.1_Specification_Publish_Subscribe_Overview">high-level</a> <a target="_blank" href="http://www.cs.rutgers.edu/~vinodg/papers/acsac2008b/acsac2008b.pdf">communication models</a> are being <a target="_blank" href="http://www2009.eprints.org/138/">researched</a> and <a target="_blank" href="http://svn.apache.org/repos/asf/incubator/shindig/trunk/features/src/main/javascript/features/rpc/">developed</a>. Even patents have been filed for methods of <a target="_blank" href="http://www.faqs.org/patents/app/20080295024">inter-window</a> and <a target="_blank" href="http://www.wipo.int/pctdb/en/wo.jsp?WO=2009036093">inter-widget</a> communication.

<img class="aligncenter" title="HTML5 logo" src="http://www.newsgeek.co.il/wp-content/uploads/2009/06/html5-logo.jpg" alt="" width="150" height="200" />

Among all of these efforts, one is catching on on a wider scale - the <strong><a target="_blank" href="http://dev.w3.org/html5/spec/Overview.html#crossDocumentMessages">HTML5 cross-document messaging specification</a></strong> aka the <strong>postMessage API</strong>.  The postMessage API has been implemented in all popular web browsers and enables secure message-oriented communication between any window objects - scripts in any window may send messages to any other window and receive messages from scripts in other windows. Security is established by two access-control mechanisms: 1) the message sender may specify the domain from which the destination window must be loaded in order for the message to be delivered, and 2) the message receiver may inspect the domain from which a message was sent and drop unwanted messages.

The <strong>problem with postMessage is that it's a very low-level and general</strong> communication mechanism which requires a lot of boiler-plate code every time it is used and even more if one wants to implement advanced communication or access-control models. Have a look at the <a target="_blank" href="http://www.whatwg.org/specs/web-apps/current-work/multipage/comms.html#introduction-5">example on using postMessage in the HTML5 spec</a> to see what I'm talking about. However, it's exactly this generality and low-levelness that makes <strong>postMessage an excellent foundation to build upon</strong>! Some of the projects I mentioned above have been working on <a target="_blank" href="http://svn.apache.org/repos/asf/incubator/shindig/trunk/features/src/main/javascript/features/rpc/">advanced</a> <a target="_blank" href="http://svn.apache.org/repos/asf/incubator/shindig/trunk/features/src/main/javascript/features/pubsub/">communication libraries</a> based on postMessage, but none that I know of are open-source and applicable in any context but rather tied to a specific application environment. Furthermore, advanced access control, fault tolerance and many other aspects of inter-window communication have been under-explored. These drawbacks were the main motivators for developing pmrpc.

<img class="aligncenter" title="pmrpc logo" src="http://code.google.com/p/pmrpc/logo?logo_id=1248529567" alt="" width="55" height="55" />

<strong><a target="_blank" href="http://code.google.com/p/pmrpc">Pmrpc</a></strong> (<em><strong>p</strong>ost<strong>M</strong>essage <strong>r</strong>emote <strong>p</strong>rocedure <strong>c</strong>all</em>) is a JavaScript library for advanced inter-window cross-domain communication. The library utilizes the postMessage API as an underlying communication mechanism and extends it to a <a target="_blank" href="http://en.wikipedia.org/wiki/Remote_procedure_call">remote procedure call (RPC) model</a> using JSON-RPC. <a target="_blank" href="http://groups.google.com/group/json-rpc/web/json-rpc-1-2-proposal">JSON-RPC</a> is a transport-independent protocol that uses the <a target="_blank" href="http://www.json.org">JSON data format</a> for formatting messages. The pmrpc library provides a simple API for exposing and calling procedures from windows or iFrames on different domains.

Here's a <strong>simple example</strong> for calling a function in another window:
<p style="padding-left:30px;">1. First, a procedure is registered for remote calls in the window or iFrame that contains the procedure:</p>

{% highlight javascript linenos %}
// [window object A] exposes a procedure in it's window
pmrpc.register( {
  publicProcedureName : "HelloPMRPC",
  procedure : function(printParam) { alert(printParam); } } );
{% endhighlight %}
 
<p style="padding-left:30px;">2. Second, the procedure is called from a remote window or iFrame by specifying the window object which contains the remote procedure, name of the procedure and parameters:</p>

{% highlight javascript linenos %}
// [window object B] calls the exposed procedure remotely, from its own window
pmrpc.call( {
  destination : window.frames["myIframe"],
  publicProcedureName : "HelloPMRPC",
  params : ["Hello World!"] } );
{% endhighlight %}
 
Furthermore, pmrpc also provides several <strong>features for advanced communication</strong>: <strong>callbacks</strong> for successful and unsuccessful calls, <strong>access control lists</strong> on both the client and server side, <strong>fault tolerance</strong> using user-defined retries and timeouts for each call, registering <strong> asynchronous procedures</strong>. The features and their usage are explained in detail on the <a target="_blank" href="http://code.google.com/p/pmrpc/wiki/PmrpcApiDocs">pmrpc API docs page</a> and a <a target="_blank" href="http://docs.google.com/present/view?id=dctznkrh_677n8758gf8">short presentation</a>, so I'll just give an <strong>example of an advanced pmrpc call</strong>:
 
<p style="padding-left:30px;">1. First, a synchronous procedure named "HelloPMRPC" accessible from windows and iframes loaded from any page on "http://www.myFriendlyDomain1.com" or exactly from the "http://www.myFriendlyDomain2.com" page is registered:</p>

{% highlight javascript linenos %}
// [window object A] exposes a procedure in it's window
var publicProcedureName = "HelloPMRPC";
var procedure = function (alertText) { alert(alertText); return "Hello!"; };
var accessControlList = {
  whitelist : ["http://www.myFriendlyDomain1.com*", "http://www.myFriendlyDomain2.com"],
  blacklist : []
};

pmrpc.register( {
  "publicProcedureName" : publicProcedureName,
  "procedure" : procedure,
  "acl" : accessControlList
} );
{% endhighlight %}
 
<p style="padding-left:30px;">2. Second, the procedure "HelloPMRPC" is called with a single positional parameter "Hello world from pmrpc client!". In case of success, the return value will be alerted while in case of errors the error description will be alerted. The call will be retried up to 15 times, waiting 1500 milliseconds between retries. Windows or iFrames loaded from any domain, except from http://evil.com, may process the call:</p>

{% highlight javascript linenos %}
// [window object B] calls the exposed procedure remotely, from its own window
var parameters = {
  destination : window.frames["targetIframeName"],
  publicProcedureName : "HelloPMRPC",
  params : ["Hello world from pmrpc client!"],
  onError : function(statusObj) {alert(statusObj.description);},
  retries : 15,
  timeout : 1500,
  destinationDomain :  {  whitelist : ["*"],  blacklist : ["http://evil.com*"] }
};

pmrpc.call(parameters);
{% endhighlight %}
 
Pmrpc is an <strong>open-source project</strong> developed by <a target="_blank" href="http://mivankovic.blogspot.com/2009/10/pmrpc.html">Marko Ivankovic</a> and <a target="_blank" href="http://www.google.com/profiles/izuzak">myself</a> and is available under the <strong>Apache v2.0 license</strong>. The library works in <strong>all web browsers</strong> that implement the postMessage API (Firefox 3, Google Chrome, Internet Explorer 8). For more information please visit the <a target="_blank" href="http://code.google.com/p/pmrpc"><strong>project homepage on Google Code</strong></a> and the <a target="_blank" href="http://code.google.com/p/pmrpc/wiki/PmrpcApiDocs"><strong>API docs page</strong></a> for a full description of the API, feature list and usage examples.

I'll end this post with a few <strong>ideas on making pmrpc even better</strong>:

1.  <strong>discovery</strong> - currently, in order to call a remote procedure one needs to have a reference to a window that exposes that procedure. Sometimes obtaining a reference to a window is not very practical and easy (or possible) and a way to find windows by various criteria is needed. For example, discover all windows with a specific name or location or discover windows that expose a procedure with a specific name.  

2. <strong>web workers</strong> - <a target="_blank" href="http://www.whatwg.org/specs/web-workers/current-work/">web workers</a> are another specification being developed in parallel with HTML5. The specification "defines an API for running scripts in the background independently of any user interface scripts". What's interesting is that the postMessage API is used to communicate with web workers and therefor pmrpc could be extended to include support for web workers.  

3. <strong>publish-subscribe mechanism</strong> - there are lots of communication models other than remote procedure call. One of the most popular models is <a target="_blank" href="http://en.wikipedia.org/wiki/Publish/subscribe">publish-subscribe</a> in which publishers publish events on channels and subscribers subscribe for events on these channels. Messages are delivered from publishers to subscribers without having a direct reference to each other.

I'd appreciate any feedback on pmrpc and our ideas for future work. What are your ideas for the future of inter-window communication? <a target="_blank" href="http://www.twitter.com/izuzak">@izuzak</a>
