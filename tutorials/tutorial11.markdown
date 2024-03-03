---
permalink: /tutorials/t11
title: Text Channels
layout: page
toc: true
---

**Note** Make sure the bot has permission to manage your server's channels.

This tutorial will walk you through the ability to create text channels with the bot.

* TOC
{:toc}

# Simplest Channel

The easiest way to create a channel is to specify only the name but no additional permissions or a parent category. To do that we will create a command that accepts a string name as input:

`/create-cmd name: channel description: Create a new channel`

Now for the command initialization code:

{% highlight golang %}
{% raw %}
{{$option := CmdOption
    Type: OptionString
    Required: true
    Name: "channel"
    Description: "Name for new channel"
}}

{{addOptions $option}}
{% endraw %}
{% endhighlight %}

And finally the command execution code:

{% highlight golang %}
{% raw %}
// Specify a new TextChannel using just the name
{{$channel := TextChannel
    Name: (getInput "channel").Value
}}

// creatChannel returns a Discord Channel object with
// additional details, or empty when it fails
{{$channel = createChannel $channel}}

{{respond (printf "Created channel %s %d"
    $channel.Name
    $channel.ID
)}}
{% endraw %}
{% endhighlight %}

In the above code, `TextChannel` is the type that allows us to specify information about the different text channel properties. By default, since we only specified the name, it will create it at the top of the channel list (outside of all the server categories) with default permissions.

