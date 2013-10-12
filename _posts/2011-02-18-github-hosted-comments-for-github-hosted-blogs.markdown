---
layout: post
title: GitHub hosted comments for GitHub hosted blogs
description: GitHub hosted comments for GitHub hosted blogs
tags: github, blog, comments, hosting
commentIssueId: 12
---

*__EDIT (January 25 2011): A while back, the GitHub team has updated the GitHub API to v3 which is now even more cool. Since the [new API](http://developer.github.com/v3/) supports fetching both the Markdown source and the HTML rendering of a comment, this eliminates the need for rendering the comments on the client-side using the Showdown library as it was done before. I've updated the blog post to reflect this and some other minor API changes.__*

Recently [switching](http://ivanzuzak.info/2011/01/02/enabling-pubsubhubbub-for-github-hosted-blogs.html) from a Wordpress.com-hosted blog to a GitHub hosted blog made me curious if blog comments could also be somehow hosted on GitHub. Why host comments on GitHub? Other than it being cool, open and hackable, it would enable that *all* content for the blog be hosted on GitHub.

So, here's what I wanted from the future commenting system:

* must be hosted completely on GitHub.
* must be tied to the GitHub repository which hosts the blog.
* must provide user login, tie each comment with the user who made the comment, and give both blog owners and users control over their comments.
* must be possible to pull the comments for a post from the system and show them on the post's Web page.
* should be easy to set-up by the blog owner and easy to use by blog readers.

These requirements ruled out using [Gists](https://gist.github.com), asking users to fork the repository and submit pull-request to a special comments file (heh, this was actually the first thing that I thought of) or using any external service for comment creation and/or hosting.

The solution I came up with is to use the [GitHub issue tracker](https://github.com/blog/411-github-issue-tracker) for creating and hosting comments and the [GitHub API](http://developer.github.com/v3/) for pulling comments from the issues into the blog's Web page <del>and [GitHub Flavored Markdown](http://github.github.com/github-flavored-markdown/) for rendering comments within the Web page using JavaScript</del>. Here's how all of this works.

<img class="aligncenter" title="GitHub hosted comments for GitHub hosted blogs" src="http://ivanzuzak.info/images/github_issues.png" alt="" width="90%" />

First, for each blog post you plan to publish, create an issue in your blog's GitHub repository issue tracker. For example, here's [the issue for this blog posts](https://github.com/izuzak/izuzak.github.com/issues/12). Notice that all issues have a common base URL (``https://github.com/izuzak/izuzak.github.com/issues/`` in my example) and a unique id (``12`` in my example). Readers of your blog will use the issue tracker if they want to leave a comment.

Second, add a link to each of your blog posts pointing to the Web page of the issue you created for comments. Since, you're probably using [Jekyll](https://github.com/mojombo/jekyll) to generate your blog, a simple way of doing this is to add the above mentioned id of the issue to the [YAML front matter](https://github.com/mojombo/jekyll/wiki/YAML-Front-Matter) block of each blog post and then add a [Liquid template](https://github.com/mojombo/jekyll/wiki/Liquid-Extensions) in your [layout file](https://github.com/mojombo/jekyll/wiki/Usage) to retrieve the id and generate the full issue URL. For example, here's the YAML front matter for the blog post your reading (``commentIssueId`` is the id of the post's issue in the repository Issue tracker):

{% highlight yaml linenos %}
---
layout: post
title: GitHub hosted comments for GitHub hosted blogs
tags: github, blog, comments, hosting
commentIssueId: 12
---
{% endhighlight %}

{% assign special = '{{page.commentIssueId}}' %}

And here's the snippet of the layout file for generating blog posts (``page.commentIssueId`` part surrounded with curly braces is the Liquid template which pulls the ``commentIssueId`` of the post):

{% highlight html linenos %}
<div id="comments">
  <h2>Comments</h2>
  <div id="header">
    Want to leave a comment? Visit <a href="https://github.com/izuzak/izuzak.github.com/issues/{{special}}"> this post's issue page on GitHub</a> (you'll need a GitHub account. What? Like you already don't have one?!).
  </div>
</div>
{% endhighlight %}

Third, add a JavaScript script to each blog posts which pulls the post's comments from the issue using the GitHub API. The [GitHub issues API](http://developer.github.com/v3/issues/) is exactly what we need here - make an <del>JSONP</del> XHR request to the API endpoint (GitHub format: `https://api.github.com/repos/:owner/:repo/issues/:number/comments`; GitHub Enterprise format: `https://:hostname/api/v3/repos/:owner/:repo/issues/:number/comments`) and you'll receive all the comments for a specific issue. For each comment you get the GitHub user id and gravatar id of the person who made the comment, the comment id in the issue, created and updated timestamps and the body of the comment. Again, since you're probably using Jekyll, it's easiest if you put the code in the layout file for blog posts. For example, here's the code for pulling in the comments for this blog post, using [jQuery](http://api.jquery.com/jQuery.ajax/) (notice again that you need a Liquid template to specify the URL of the issue for which you want to pull the comments):

{% highlight html linenos %}
<script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1/jquery.min.js"></script>

<script type="text/javascript">
  function loadComments(data) {
   // ...
  }

  $.ajax("https://api.github.com/repos/izuzak/izuzak.github.com/issues/{{page.commentIssueId}}/comments", {
    headers: {Accept: "application/vnd.github.full+json"},
    success: function(msg){
      loadComments(msg);
   }
  });
</script>
{% endhighlight %}

But wait, doesn't the [same-origin policy](http://en.wikipedia.org/wiki/Same_origin_policy) (SOP) apply when making an XHR request to the GitHub API endpoint from the blog's page? Yes, yes it does. And until recently, the only way to get around SOP for accessing the GitHub API was to use [JSONP](http://en.wikipedia.org/wiki/JSONP). However, using JSONP disables the possibility of setting HTTP request headers which we need to correctly set the desired API response media type via the ``Accept`` header (see code above). However, as of recently, GitHub supports the [CORS specification](http://www.w3.org/TR/cors/) which enables cross-origin requests. The [way in which GitHub enables CORS for its API](http://developer.github.com/v3/#cross-origin-resource-sharing) is by requiring API users to register the Web sites that access the API as OAuth applications. Therefore, in order to enable CORS for a GitHub-hosted blog, you need to go to the [GitHub form for registering OAuth applications](https://github.com/account/applications/new) and create a new application by specifying your blog's URL in the Main URL field and the Callback URL field. Notice that this approach works (and is the same) both for blogs on GitHub subdomains (e.g. ``izuzak.github.com``) and for blogs hosted on GitHub that have a [custom domain](http://pages.github.com/#custom_domains) (e.g. ``http://ivanzuzak.info``).

<img class="aligncenter" title="GitHub OAuth application registration form" src="http://ivanzuzak.info/images/github_oauth.png" alt="" width="90%" />

Fourth, insert the pulled comment data into the blog post's HTML <del>and render the body of the comments using [GitHub Flavored Markdown](http://github.github.com/github-flavored-markdown/). Inserting the comment data into the DOM is easy once you decide which data you want to display. However, since issue comments may be written in GitHub Flavored Markdown, some JavaScrip code is needed to translate the comment to HTML. Fortunately, GitHub has a [JavaScript library](https://github.com/github/github-flavored-markdown) for that also - a modified version of the [Showdown library](https://github.com/coreyti/showdown) for converting Markdown into HTML.</del> Since the pulled data contains a HTML rendering of the comment's Markdown source, there's no need to do any processing of the comment text. Again, it's easiest if you put the code in the layout file for blog posts. For example, here's the code which I use for inserting comments:

{% highlight html linenos %}
<script type="text/javascript" src="http://datejs.googlecode.com/svn/trunk/build/date-en-US.js"></script>

<script type="text/javascript">

  function loadComments(data) {
    for (var i=0; i<data.length; i++) {
      var cuser = data[i].user.login;
      var cuserlink = "https://www.github.com/" + data[i].user.login;
      var clink = "https://github.com/izuzak/izuzak.github.com/issues/{{page.commentIssueId}}#issuecomment-" + data[i].url.substring(data[i].url.lastIndexOf("/")+1);
      var cbody = data[i].body_html;
      var cavatarlink = data[i].user.avatar_url;
      var cdate = Date.parse(data[i].created_at).toString("yyyy-MM-dd HH:mm:ss");

      $("#comments").append("<div class='comment'><div class='commentheader'><div class='commentgravatar'>" + '<img src="' + cavatarlink + '" alt="" width="20" height="20">' + "</div><a class='commentuser' href=\""+ cuserlink + "\">" + cuser + "</a><a class='commentdate' href=\"" + clink + "\">" + cdate + "</a></div><div class='commentbody'>" + cbody + "</div></div>");
    }
  }
</script>
{% endhighlight %}

Also notice that I use the [DateJS library](http://www.datejs.com/) to pretty-print the comment dates and that I fetch the Gravatar images of users who made the comments.

Finally, add CSS rules to make the comments look pretty. And if you're extra geeky and love GitHub like me, make the comments look like GitHub code comments:

<img class="aligncenter" title="GitHub hosted comments for GitHub hosted blogs" src="http://ivanzuzak.info/images/github_comments.png" alt="" width="90%" />

And that's it! Creating the system was really easy - GitHub already had all the pieces necessary (Issue tracker, API, OAuth application registration form<del>, Markdown JavaScript library</del>) and all I needed was a bit of glue to tie all of them together. I'm using this as the commenting system for my blog until some serious problem turns up. I like how the system integrates with GitHub login, how all blog comments are visible on the issue page and the overall simplicity of using the system. What I didn't find an easy way of doing is creating issue comments directly from the a blog post's Web page, without making the user navigate to the Issue tracker page (really hard to do without an external service since the blog is a static site). Other than that, I'm not sure what will happen when spam bots start invading GitHub issue trackers.

You can see the complete code I'm using for the commenting system in the following files: [general layout for blog posts](https://github.com/izuzak/izuzak.github.com/blob/master/_layouts/post.html#L24-L55) and [CSS rules](https://github.com/izuzak/izuzak.github.com/blob/master/css/screen.css#L228-L395). Let me know if you find a better system for hosting blog comments on GitHub or improving this one. [@izuzak](http://www.twitter.com/izuzak)
