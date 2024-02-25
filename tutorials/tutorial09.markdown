---
permalink: /tutorials/t09
title: Functions
layout: page
toc: true
---

This tutorial will walk you through creating functions and calling them using `exec`.

Although normal triggers execute when certain guild events happen, functions can only be triggered manually. There is also a higher limit to the number of functions you can create - 20 by default versus 3 for every other trigger type.

* TOC
{:toc}

# Create

Before you can use `exec`, you first need to create a function that you can call. To do this you can use the `/create-trigger` command and select "Function (exec)" for its type. Then you can specify the code just as you would for other trigger types.

Function (exec) is a special type of trigger. Not only can you have more of them (20 versus the normal 3), but they must be triggered manually through a call to `exec`. When the code executes, its `context` is derived from the caller's context. This means it inherits the same triggering user, channel, message, etc. On top of this, it provides a new context input: `context.ExecData`. This allows the calling code to pass custom data into the function.

# Exec

Here is the type signature for the exec function:

**exec function data delay**

`function` needs to be the name as a string. Whatever is passed into `data` gets put into `context.ExecData` for the function to use. `delay` allows you to specify a number (in seconds) from 0 to 60. 0 means "execute immediately."

# Example: Exec with delay

Here is the function (named "PrintData") code:

{% highlight golang %}
{% raw %}
// Custom input will be placed into context.ExecData
{{$data := context.ExecData}}

// We inherit the channel from caller
{{sendMessage context.Channel.ID (print "Function called with data: " $data)}}
{% endraw %}
{% endhighlight %}

Then we create a slash command that will call it:

{% highlight golang %}
{% raw %}
// Add 5 second delay
// currentTime is a builtin function
{{exec "PrintData" currentTime 5}}

// We could also call it with nil if we wanted to
// {{exec "PrintData" nil 5}}

{{respond "Called PrintData with delay"}}
{% endraw %}
{% endhighlight %}

Here is what it could look like:

![peaches](/peaches-bot.docs/assets/t09/print.png)