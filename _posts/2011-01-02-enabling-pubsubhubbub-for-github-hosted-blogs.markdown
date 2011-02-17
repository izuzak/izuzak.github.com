---
layout: post
title: Enabling PubSubHubBub for GitHub hosted blogs
tags: github, pubsubhubbub, feed, atom, webhooks, appengine
commentIssueId: 11
---

I've recently switched from a [Wordpress.com hosted blog](http://izuzak.wordpress.com/) to a [GitHub hosted blog](http://ivanzuzak.info) on a custom domain using [GitHub Pages](http://pages.github.com/). And one of the first things I wanted to enable for the new blog was [PubSubHubBub](http://code.google.com/p/pubsubhubbub/) (PSHB) support. Generally, in order to enable PSHB for a blog, two things need to be done:

1) add a `<link rel="hub" href="http://url.of.a.pshb.hub" />` to the blog's ATOM feed, linking to a PSHB hub, e.g. the reference hub running on appengine `http://pubsubhubbub.appspot.com` (see [PSHB specification Section 5. - Discovery](http://pubsubhubbub.googlecode.com/svn/trunk/pubsubhubbub-core-0.3.html#discovery))

2) every time a new blog post is created or an old blog post is updated - notify the hub from step 1) by making a HTTP POST request with the body containing the blog's ATOM feed URL (see [PSHB specification Section 7.1. - New Content Notification](http://pubsubhubbub.googlecode.com/svn/trunk/pubsubhubbub-core-0.3.html#anchor9))

For GitHub hosted blogs, step 1) isn't a problem as it only requires you to insert one line of text into your blog's ATOM XML feed. However, step 2) is a problem. The good news is that **each GitHub Pages site is also a GitHub repository and GitHub supports service hooks**, predefined services which GitHub notifies each time the repository is pushed to. The bad news is that, although many services like Twitter and Campfire are supported, PubSubHubBub hubs are not supported. So basically, you need to make a custom service. Now, a special type of service hooks are **generic [post-receive hooks](http://help.github.com/post-receive-hooks/), user-specified URLs (also called [webhooks](http://wiki.webhooks.org/))** to which GitHub makes a HTTP POST request each time the repository is pushed to. And now for more bad news. First, HTTP POST requests made to post-receive hook URLs have a predefined and non-changeable message body. Therefore, because a PSHB notify request must contain certain parameters in the body of the HTTP POST request, there is no way of specifying these parameters since the body is predefined by GitHub. Second, requests made to post-receive hook URLs are stripped from the URL query part. Therefore, even if PSHB supported passing notification parameters through the query part of the request URL, GitHub wouldn't allow that either. I've asked about this issue on the GitHub forum and [got a semi-satisfying response](http://support.github.com/discussions/post-receive-issues/149-the-query-part-of-post-receive-urls-is-disregarded-before-making-the-post-request).

**Until there's a better solution, here's an ugly-hack solution**. I created an AppEngine proxy service, to be used as a post-receive hook, which makes PSHB notifications only to the [reference PSHB hub](http://pubsubhubbub.appspot.com/) when a POST request is made to the service. Since GitHub doesn't allow query parameters in the URL, the proxy service will expect the blog's ATOM feed URL to be passed as _the suffix of the path part of the URL_ (e.g. instead of making a request to `http://www.service.com?blogUrl=http://some.url.com`, a request would be made to `http://www.service.com/http://some.url.com`).

So, here's a **step-by-step guide for enabling PubSubHubBub for your GitHub hosted blog**:

1) Add the following lines to your blog's ATOM feed XML:

>     <link href="http://url.of.your.blogs.atom.feed.xml" rel="self"/>
>     <link href="http://pubsubhubbub.appspot.com/" rel="hub"/>

where `http://url.of.your.blogs.atom.feed.xml` is what it says it is - the URL of your blog's ATOM feed (e.g. mine is `http://ivanzuzak.info/atom.xml`). If this line is already present in your blog's ATOM feed, you don't have to add it again. Naturally, you will perform this step by editing your atom.xml source file and push it to the GitHub repository hosting your blog. You can see my atom.xml source [here](https://github.com/izuzak/izuzak.github.com/blob/master/atom.xml).

2) Open the admin page for the GitHub project hosting your blog and navigate to the Service hooks tab. Now, add a new post-receive hook with the following URL:

>     http://urlreq.appspot.com/pshbpinggae/http://url.of.your.blogs.atom.feed.xml

where, `urlreq.appspot.com` is the AppEngine proxy service I mentioned earlier and, again, `http://url.of.your.blogs.atom.feed.xml` is what it says it is - the URL of your blog's ATOM feed. For example, the post-receive hook URL for my blog is `http://urlreq.appspot.com/pshbpinggae/http://ivanzuzak.info/atom.xml`. Passing parameters this way is really ugly, but currently there's no better way of doing it.

<img class="aligncenter" title="PubSubHubBub support for GitHub hosted blogs" src="http://ivanzuzak.info/images/pshb_github_gae_ss.png" alt="" width="100%" />

Now every time you push a new blog post to the blog's GitHub repository, GitHub will make a POST request to the urlreq AppEngine proxy service, the service will notify the reference PSHB hub, the hub will fetch your blog's ATOM feed and notify all subscribers of the new feed entry. And if you make a commit the the repository which doesn't contain a new blog post, the service will still notify the hub, but the hub is smart enough to detect that no new entries are present in the ATOM feed so no subscribers will be notified.

GitHub pages are really cool for hosting blogs and GitHub should do more in supporting them - it would be cool if a PubSubHubBub service hook was added to the list of Service hooks or if generic post-receive hooks supported specifying custom parameters for the hooks (either through the URL query part or through the HTTP request body). [@izuzak](http://www.twitter.com/izuzak)
