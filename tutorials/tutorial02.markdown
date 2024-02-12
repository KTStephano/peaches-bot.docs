---
permalink: /tutorials/t02
title: Data Containers
layout: page
toc: true
---

This tutorial will walk you through the 3 main containers available through the bot: Array, Map and SMap.

* TOC
{:toc}

# Array

The array works almost the same as in other languages. It is capable of being dynamically added to up to 1000 elements to prevent bot abuse.

Creating an array is done as follows:

{% highlight golang %}
{% raw %}
{{$a := Array 1 2 "hello" false}} // Array with 4 elements
{{$a = Array}} // Empty array
{% endraw %}
{% endhighlight %}


It's also possible to iterate over them using `range`.

{% highlight golang %}
{% raw %}
{{$a := Array "a" "b" "c" "d" "e" "f"}}
{{range $elem := $a}} // stores each element of $a in $elem
    {{respond $elem}}
    {{sleep 1}}
{{end}}
{% endraw %}
{% endhighlight %}

Since arrays are dynamic, it's also possible to append one or more values to an existing array. An important thing to keep in mind is that while an array will try to use existing space to append new elements, if it runs out of space it will perform a resize+copy and return a new array. For this reason, it's important to capture the return value of `.Append`. Here is an example of some code that appends multiple values to the array and then prints the array and its length.

{% highlight golang %}
{% raw %}
{{$a := Array}}
{{$a = $a.Append 1 2 3 "true"}}
{{$a = $a.Append 4 5 false 6 7}}
{{respond (printf "%s %d" $a (len $a))}}
{% endraw %}
{% endhighlight %}

With an existing array, it's possible to create a **slice** of the array. When created, a slice is a view into the original array with a modified start and/or end.

{% highlight golang %}
{% raw %}
{{$a := Array 1 2 3 4 5 6 7 8 9 10}}
// Prints entire array
{{respond (slice $a)}}
{{sleep 1}}
// Prints the array starting at element 4
{{respond (slice $a 4)}}
{{sleep 1}}
// Prints the array starting at element 4 and ending at element 6 (7 not included)
{{respond (slice $a 4 7)}}
{% endraw %}
{% endhighlight %}

# Map and SMap

Both map and smap are associative containers. The only difference is that map allows for its keys to be any comparable type, whereas smap only allows its keys to be strings. Maps and smaps also internally modify their structure and do not return copies when they run out of space, unlike arrays/slices of arrays. Like arrays, they are limited to 1000 elements to prevent bot abuse.

Creating a map is done as follows:

{% highlight golang %}
{% raw %}
{{$sm := SMap "hello" 1 "world" 2 "!" 3}}
{% endraw %}
{% endhighlight %}

Iterating over the map/smap can be done with range, only this time it returns both the key and value.

{% highlight golang %}
{% raw %}
{{$sm := SMap "hello" 1 "world" 2 "!" 3}}
{{range $k, $v := $sm}}
    {{respond (printf "%s: %d" $k $v)}}
    {{sleep 1}}
{{end}}
{% endraw %}
{% endhighlight %}

Adding and removing elements can be done with `.Insert` and `.Remove`.

{% highlight golang %}
{% raw %}
{{$sm := SMap "hello" 1 "world" 2 "!" 3}}
{{$sm.Insert "earth" true}}
{{respond $sm}}
{{sleep 1}}

{{$sm.Remove "hello"}}
{{respond $sm}}
{% endraw %}
{% endhighlight %}

# Contains

Arrays and Maps/SMaps can all be searched using the `contains` function. This function is not a member of the types themselves, so you have to pass in the container as an argument.

{% highlight golang %}
{% raw %}
{{$sm := SMap "hello" 1 "world" 2 "!" 3}}
{{respond (contains $sm "world")}} // true
{{respond (contains $sm "earth")}} // false

{{$a := Array 1 2 3 4 5}}
{{respond (contains $a 1)}} // true
{{respond (contains $a 7)}} // false
{% endraw %}
{% endhighlight %}