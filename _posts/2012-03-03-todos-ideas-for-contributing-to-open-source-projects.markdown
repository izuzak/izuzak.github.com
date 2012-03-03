---
layout: post
title: Forgotten TODOs: ideas for contributing to open-source projects
tags: open-source, ideas, programming, grep
commentIssueId: 14
---

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

These notes are focused more on improving the internal operation of the system rather than on externally observable features, and include anything from checking specific algorithm edge-cases to larger refactoring ideas.
And as time goes by, these __TODOs are often forgotten since they are not systematically tracked and more effort is put into developing new features and fixing bugs__.
So, if you really want to get your hands dirty, TODOs are a good place to start looking for work.
At first, project owners and maintainers will often be cautious about letting you hack into system internals, but will generally be very grateful if you really want to fix those forgotten TODOs.

Here's the number of TODOs for the top 15 watched projects on GitHub:

<table border="1">
  <thead>
    <tr>
      <th>Project name</th>
      <th>Number of TODOs</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>bootstrap</td>
      <td>7</td>
    </tr>
    <tr>
      <td>nodejs</td>
      <td>904 (many of these are v8 TODOs)</td>
    </tr>
    <tr>
      <td>rails</td>
      <td>77</td>
    </tr>
    <tr>
      <td>jquery</td>
      <td>7</td>
    </tr>
    <tr>
      <td>html5-boilerplate</td>
      <td>2</td>
    </tr>
    <tr>
      <td>homebrew</td>
      <td>22</td>
    </tr>
    <tr>
      <td>spoon-knife</td>
      <td>0</td>
    </tr>
    <tr>
      <td>impress.js</td>
      <td>0</td>
    </tr>
    <tr>
      <td>backbone</td>
      <td>4</td>
    </tr>
    <tr>
      <td>diaspora</td>
      <td>16</td>
    </tr>
    <tr>
      <td>three20</td>
      <td>25</td>
    </tr>
    <tr>
      <td>devise</td>
      <td>2</td>
    </tr>
    <tr>
      <td>jquery-mobile</td>
      <td>60</td>
    </tr>
    <tr>
      <td>three.js</td>
      <td>43</td>
    </tr>
    <tr>
      <td>express</td>
      <td>3</td>
    </tr>
  </tbody>
</table>

Of course, in some projects you won't find any TODOs, in others you'll find false positives and TODOs from 3rd party libraries, but in general - forgotten TODOs are a cool way of finding ideas for contributing to open-source projects.
