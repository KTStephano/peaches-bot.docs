---
permalink: /tutorials/t03
title: Data Containers
layout: page
toc: true
---

This tutorial will walk you through the 3 main containers available through the bot: Array and Map.

* TOC
{:toc}

# Array

The array works almost the same as in other languages including 0-indexing. It is capable of being dynamically added to up to 1000 elements to prevent bot abuse.

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
{{end}}
{% endraw %}
{% endhighlight %}

Since arrays are dynamic, it's also possible to append one or more values to an existing array. An important thing to keep in mind is that while an array will try to use existing space to append new elements, if it runs out of space it will perform a resize+copy and return a new array, leaving the original array unchanged. For this reason, it's important to capture the return value of `append`. Here is an example of some code that appends multiple values to the array and then prints the array and its length.

{% highlight golang %}
{% raw %}
{{$a := Array}}
{{$a = append $a 1 2 3 "true"}}
{{$a = append $a 4 5 false 6 7}}

// len gets the size of strings, maps, arrays
{{respond (printf "%s %d" $a (len $a))}}
{% endraw %}
{% endhighlight %}

With an existing array, it's possible to create a **slice** of the array. When created, a slice is a view into the original array with a modified start and/or end.

{% highlight golang %}
{% raw %}
{{$a := Array 1 2 3 4 5 6 7 8 9 10}}

// Prints the array starting at index 4
{{respond (slice $a 4)}}

// Prints the array starting at element 4 and ending at element 6 (7 not included)
{{respond (slice $a 4 7)}}
{% endraw %}
{% endhighlight %}

It's also possible to overwrite existing values in an array with `insert`. Here is an example:

{% highlight golang %}
{% raw %}
{{$a := Array 1 2 3 4 5 6 7 8 9 10}}
// overwrites index 1 with "hello"
{{$a = insert $a 1 "hello"}}
// array is now [1 "hello" 3 4 ...]
{% endraw %}
{% endhighlight %}

# Map

A map is a general-purpose associative container. It allows for key-value pairs to be stored together for fast lookup, insert/overwrite and removal. This is similar to an array since they map integer indices to a value, except with a general map you can use any comparable type (bool, int, float, string, pointer, etc.). Like arrays, they are limited to 1000 elements to prevent bot abuse.

Creating a map is done as follows:

{% highlight golang %}
{% raw %}
{{$sm := Map "hello" 1 "world" 2 "!" 3}}
{% endraw %}
{% endhighlight %}

Iterating over the map/smap can be done with range, only this time it returns both the key and value.

{% highlight golang %}
{% raw %}
{{$sm := Map "hello" 1 "world" 2 "!" 3}}
{{$msg := ""}}
{{range $k, $v := $sm}}
    {{$msg = printf "%s: %d\n" $k $v}}
{{end}}
{{respond $msg}}
{% endraw %}
{% endhighlight %}

Adding and removing elements can be done with `insert` and `remove`.

{% highlight golang %}
{% raw %}
{{$sm := Map "hello" 1 "world" 2 "!" 3}}
{{$sm = insert $sm "earth" true}}
{{respond $sm}}

{{$sm = remove $sm "hello"}}
{{respond $sm}}
{% endraw %}
{% endhighlight %}

Notice that the result of `insert` and `remove` is used to overwrite the value of variable `$sm`. Most of the time the existing map will be used and modified, but there is one notable case where the bot will create a new map and return it instead. This is if you try to modify one of the builtin bot runtime maps that are given to your code as an input. Those are immutable, so the bot first needs to make a full copy of the map so you can modify it.

For keys that are strings and only use alphanumeric characters (where the first character is alphabetical), you have access to special . syntax for lookup as follows:

{% highlight golang %}
{% raw %}
{{$sm := Map "hello" 1 "world" 2 "earth" true}}
{{respond $sm.hello}}  // shows 1
{{respond $sm.earth}}  // shows true
{{respond $sm.planet}} // shows empty (nil) since planet is not a string key in the map
{% endraw %}
{% endhighlight %}

# Contains

Arrays, maps and strings can all be searched using the `contains` function. In the case of strings it works as a substring search.

{% highlight golang %}
{% raw %}
{{$sm := Map "hello" 1 "world" 2 "!" 3}}
{{respond (contains $sm "world")}} // true
{{respond (contains $sm "earth")}} // false

{{$a := Array 1 2 3 4 5}}
{{respond (contains $a 1)}} // true
{{respond (contains $a 7)}} // false

{{respond (contains "hello world" "world")}} // true
{{respond (contains "hello world" "earth")}} // false
{% endraw %}
{% endhighlight %}

[Next <- Tutorial 4: Triggers](/tutorials/t04)