---
layout: post
title: "Forgotten TODOs: ideas for contributing to open-source projects"
description: "Forgotten TODOs: ideas for contributing to open-source projects"
tags: open-source, ideas, programming, grep
commentIssueId: 14
---

*__EDIT (March 08 2012)__:* This post got a lot of feedback on [Hacker News](http://news.ycombinator.com/item?id=3660137) and [Reddit](http://www.reddit.com/r/programming/comments/qg3nz/ideas_for_contributing_to_opensource_projects/).
I'd like to thank all the people on their positive and constructive thoughts and address some of the feedback.

First, there was some really good advice on advanced usage of TODOs, such as using tools for tracking them systematically and using different TODO tags (such as TODO, FIXME, TBD, and XXX) to indicate the relevance/urgency of the TODO.
Since a lot of people mentioned that they also use FIXMEs, I've also included their count in the table in the post.

However, the comments I liked the most were the ones that suggested other generic ideas for contributing to open-source projects, especially the ones that mentioned writing documentation and unit tests.
I can't stress enough how good advice this is.

There's also been some negative feedback, mainly focused on explaining that newcomers should not target TODOs because they are usually either very hard to fix, will have little value if fixed, or are not meant to be fixed by someone who is not the project maintainer.
In addition, some comments say that newcomers should focus on fixing bugs that they have experienced themselves and implementing features that mean something to them ("scratch your own itch").
While I agree with these points and have tried to make them clearer in the post, I have to stress that the main point of this post is to help people get ideas when they don't have ideas of their own.
Therefore, regardless of their programming skills and the impact of fixing a TODO, I do think that looking through TODOs will help in generating ideas.

<hr/>

I often talk to students that want to contribute to open-source projects, but just don't have an idea what to work on.
Here's a tip if you're in a similar situation (e.g. you want to apply for [GSOC](http://code.google.com/soc/)) :

{% highlight bash linenos %}
git clone repository_url_of_some_open_source_project target_directory
grep -RIn TODO target_directory/*
{% endhighlight %}

So, __find the URL of the repository project you want to contribute to, checkout the repository using git/mercurial/svn and then find all the TODOs in the source code using grep__.
The `-RIn` flags will tell grep to do a recursive search (-R), skip binary files (-I) and include line numbers (-n) for results.
And that's it!
Go through the results and pick whatever you find interesting and within your skill range.

Most open-source projects have issue trackers for organizing bug reports and feature requests that need to be worked on.
However, some projects don't use issue trackers, and there's an important difference between issues in these trackers and what you'll find by digging through the source code.
Issues are commonly a result of _using_ a system or library, they are created by users, and focus on bugs and features.
TODOs on the other hand are a result of _system development_ and are created by developers.
Software developers often make notes (TODOs) of things that should be fixed or could be done in a better way, but don't have the time to do it right away.

These notes are focused more on improving the internal operation of the system and code quality, rather than on externally observable features, and include anything from checking specific algorithm edge-cases to larger refactoring ideas.
And as time goes by, these __TODOs are often forgotten since they are not systematically tracked and more effort is put into developing new features and fixing bugs__.
So, if you really want to get your hands dirty, TODOs are a good place to start looking for work.
At first, project owners and maintainers will often be cautious about letting you hack into system internals and refactor, but will generally be very grateful if you really want to fix those forgotten TODOs.
In a way, it's often a bit harder to start working on a TODO than on an tracked issue.

Here's the number of TODOs and FIXMEs for the top 15 watched projects on GitHub:

<table border="1">
  <thead>
    <tr>
      <th>Project name</th>
      <th>Number of TODOs + FIXMEs</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>bootstrap</td>
      <td>7+0</td>
    </tr>
    <tr>
      <td>nodejs</td>
      <td>904+278 (many of these are v8 TODOs)</td>
    </tr>
    <tr>
      <td>rails</td>
      <td>77+55</td>
    </tr>
    <tr>
      <td>jquery</td>
      <td>7+0</td>
    </tr>
    <tr>
      <td>html5-boilerplate</td>
      <td>2+0</td>
    </tr>
    <tr>
      <td>homebrew</td>
      <td>22+6</td>
    </tr>
    <tr>
      <td>spoon-knife</td>
      <td>0+0</td>
    </tr>
    <tr>
      <td>impress.js</td>
      <td>0+0</td>
    </tr>
    <tr>
      <td>backbone</td>
      <td>4+0</td>
    </tr>
    <tr>
      <td>diaspora</td>
      <td>16+0</td>
    </tr>
    <tr>
      <td>three20</td>
      <td>25+0</td>
    </tr>
    <tr>
      <td>devise</td>
      <td>2+0</td>
    </tr>
    <tr>
      <td>jquery-mobile</td>
      <td>60+0</td>
    </tr>
    <tr>
      <td>three.js</td>
      <td>43+6</td>
    </tr>
    <tr>
      <td>express</td>
      <td>3+0</td>
    </tr>
  </tbody>
</table>

Of course, in some projects you won't find any TODOs, in others you'll find false positives and TODOs from 3rd party libraries, but in general - forgotten TODOs are a cool way of finding ideas for contributing to open-source projects.
