---
layout: post
title: "The power of Underscore.js"
tags: [fp, javascript, underscore]
---

[Underscore.js](http://underscorejs.org) is a powerful JavaScript
library that provides useful functional programming helpers. Why useful?
Let see in a couple of examples below but in a few words it allows you to
make your code more clear and compact.

First example is filtering of the list. Let say you have trivial list of
integer numbers and need to find even ones. I am sure every developer
knows how to do it using `for` loop:

{% highlight javascript %}
var numbers = [1, 2, 3, 4, 5, 6];
var evens = [];
for (var i = 0; i < numbers.length; i++) {
  if (numbers[i] % 2 == 0) {
    evens.push(numbers[i]);
  }
}
{% endhighlight %}

Compare it with implementation using Underscore's `filter` function:

{% highlight javascript %}
var numbers = [1, 2, 3, 4, 5, 6];
var evens = _.filter(numbers, function(num) { 
  return num % 2 == 0; 
});
{% endhighlight %}

Second example is optimization of the search performed on some list. In
some situation when you need to repeat array search in a cycle it make
sense to convert it into hash map. Let's say you have a list of integer
numbers and you do as usual:

{% highlight javascript %}
var numbers = [10, 11, 12, 25, 36];
var map = {};
for (var i = 0; i < numbers.length; i++) {
  map[numbers[i]] = numbers[i];
}
{% endhighlight %}

See how it's trivial with `indexBy` function:

{% highlight javascript %}
var numbers = [10, 11, 12, 25, 36];
var map = _.indexBy(numbers, function(n) { return n; });
{% endhighlight %}

So if you want to write less code to achive result, then Underscore.js
is your friend :-) See [Underscore.js web site](http://underscorejs.org)
and learn new useful tricks. Another good source of documentation for
Undescore and many other libraries is [DevDocs](http://devdocs.io)
