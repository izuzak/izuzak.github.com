---
layout: post
title: Named arguments in JavaScript
---

While working on another project (<a title="pmrpc" href="http://code.google.com/p/pmrpc" target="_blank">pmrpc</a>, which I will write about soon), the implementation of a feature came down to calling JavaScript functions with <a href="http://en.wikipedia.org/wiki/Named_parameter" target="_blank">named arguments</a>. Unfortunately, the JavaScript language (or EcmaScript to be exact) <a href="http://ajaxian.com/archives/chrome-extension-api-how-we-wish-we-have-named-parameters">doesn't support named arguments</a>, rather only positional arguments.

<a href="http://www.javascriptkit.com/javatutors/namedfunction.shtml" target="_blank">Usually</a>, JavaScript developers get around this by defining functions to accept a single object argument which acts like a container for all other named parameters:

{% highlight javascript linenos %}
// params is a container for other named parameters
function callMe(params) {
  var param1 = params['param1'];
  var param2 = params['param2'];
  var param3 = params['param3'];

  // do something...
}

var result = callMe({ 'param3' : 1, 'param1' : 2, 'param2' : 3 });
{% endhighlight %}

This solution works nicely when you are in the position to modify every function to be called this way. However, this is not always the case (e.g. when using external libraries), and also isn't as readable as having a proper parameter list, and introduces code overhead in every function that implements this principle. What I needed for my project is something that enables calling any JavaScript function with named arguments, without modifying the function itself.

What I came up with (code presented below) is based on the following:
<ol>
	<li>using the <em>toString </em>method of the Function prototype, it is possible to retrieve a string representation of any function definition</li>
	<li>from the string representation, it is possible to parse the number, order and names of function parameters</li>
	<li>from a dictionary of named arguments, it is possible to construct an array of arguments in the correct order using the parsed representation</li>
	<li>using the <em>apply </em>method of the Function prototype, it is possible to dynamically call a function, passing an array of arguments instead of an argument list.</li>
</ol>

{% highlight javascript linenos %}
// calls function fn with context self and parameters defined in dictionary namedParams
function callWithNamedArgs( fn, self, namedParams ) {
  // get string representation of function
  var fnDef = fn.toString();

  // parse the string representation and retrieve order of parameters
  var argNames = fnDef.substring(fnDef.indexOf("(")+1, fnDef.indexOf(")"));
  argNames = (argNames === "") ? [] : argNames.split(", ");
  var argIndexes = {};
  for (var i=0; i<argNames.length; i++) {
    argIndexes[argNames[i]] = i;
  }

  // construct an array of arguments from a dictionary
  var callParameters = [];
  for (paramName in namedParams) {
    if (argIndexes[paramName]) {
      callParameters[argIndexes[paramName]] = namedParams[paramName];
    }
  }

  // invoke function with specified context and arguments array
  return fn.apply(self, callParameters);
}

// example function
function callMe(param1, param2, param3) {
  // do something ...
}

// example invocation with named parameters
var result = callWithNamedArgs(callMe, this, { 'param3' : 1, 'param1' : 2, 'param2' : 3 });
{% endhighlight %}

This could also have been implemented as a Function prototype method and be called directly on a function object, which would simplify usage since the target function parameter wouldn't have to be passed. But, if the <em>CallWithNamedArgs</em> function indeed was implemented prototype function, could the context parameter be removed also? I.e. is it possible to retrieve the current execution context (<em>this </em>object) of the target function within the <em>CallWithNamedArgs </em>method? The problem is that the execution context within the <em>CallWithNamedArgs method </em>is the Function object on which the <em>CallWithNamedArgs </em>method was called. But what we need is the execution context of the target function, which is the Function object on which the <em>CallWithNamedArgs </em>method was called. That is, what we need is the current execution context of the parent object. Any ideas? <a href="http://twitter.com/izuzak">@izuzak</a>