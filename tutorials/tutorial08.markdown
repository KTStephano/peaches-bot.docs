---
permalink: /tutorials/t08
title: Threads
layout: page
toc: true
---

This tutorial will walk you through the process of managing basic, message and forum threads. Be sure to check the bot's permissinos to make sure it is able to post and create threads.

* TOC
{:toc}

# Create

All threads are created using the `createThread` function. Here is its signature:

**createThread channelID thread**

`channelID` determines which channel the thread is going to be created in. `thread` is either a `Thread` or `ForumThread` object (both discussed below) and contains information that the function needs to know about the thread's initial settings.

When this function succeeds it returns a [*Channel](/docs#contextchannel) object (Discord represents threads as channels), and if it fails it returns empty (nil).

# Basic Threads

The first type we'll look at is the basic thread. This is a thread created inside of a text channel, and its access can be set to either public or private. Private means that until a user is invited (such as if they are @ mentioned), they will not see the thread or its contents or members.

Here is a function that creates a new thread in the channel that the command is executed in.

**command initialize code**

{% highlight golang %}
{% raw %}
{{$option1 := CmdOption
    Type: OptionString 
    Required: true
    Name: "title" 
    Description: "Title of the thread"
}}

{{$option2 := CmdOption
    Type: OptionBoolean
    Required: true
    Name: "private"
    Description: "Thread private setting (true/false)"
}}

{{addOptions $option1 $option2}}
{% endraw %}
{% endhighlight %}

**command execute code**

{% highlight golang %}
{% raw %}
{{$thread := Thread
    Title: (getInput "title").String // title of the thread
    Private: (getInput "private").Bool // true if the thread is private
}}

{{$created := createThread context.Channel.ID $thread}}
// if statement checks if create returned a valid object. If not
// it means the bot couldn't create the thread
{{if $created}}
    // send message will mention the user in the newly-created thread
    {{sendMessage $created.ID context.Member}}
    {{respondPrivate "Done!"}}
{{else}}
    {{respondPrivate "Failed"}}
{{end}}
{% endraw %}
{% endhighlight %}

This code uses the `Thread` type. This type allows you to specify a few thread settings that Discord can set when creating it. Here's an example of what it could look like when using this command:

![basic_thread](/assets/t08/basic_thread.png)

# Message Thread

Another type of thread is the message thread. This thread is always public within the context of the channel it's created in. It also has another unique property of being attached to an existing message. The use case is for when someone posts something and there is a need to create a sub-channel attached to it in order to redirect conversation. Here is an example of a trigger that executes whenever a message is created in a channel and has the bot automatically create a new message thread.

Trigger type: **message created**

{% highlight golang %}
{% raw %}
// Change the ID to your own channel
{{if eq context.Channel.ID 1197294863885013013}}
    {{$thread := Thread
        Title: "Message Thread"
        // context.Message contains the message that triggered this code
        MessageID: context.Message.ID
    }}
    {{createThread context.Channel.ID $thread}}
{{end}}
{% endraw %}
{% endhighlight %}

Here is an example of what it could look like when a user sends a message into the right channel:

![message_thread](/assets/t08/message_thread.png)

Here are the all the available Thread fields:

**type Thread**

.Title | String | **required** | Title of the thread
.AutoArchiveDuration | Int | **optional** | Duration in minutes before Discord auto archives the thread
.Private | Bool | **optional** | (Only valid for basic thread) True if thread permission set to private
.Slowmode | Int | **optional** | Duration in seconds where someone must wait before sending a new message
.MessageID | ID | **optional** | (Only valid for message thread) message ID to attach the thread to

# Forum Threads

The final type of thread are those created inside of forum channels. These have some of the same configuration options as regular threads, but they have the added requirement of needing content to go along with the title (serves as the forum post body). In addition to this, forum threads can be associated with a set of tags (max 5 per forum thread).

Creating forum threads is done with `createThread` just as before, but the thread type is created by using `ForumThread`. Here is an example of a command that creates a new forum post:

**command initialize code**

{% highlight golang %}
{% raw %}
{{$option1 := CmdOption
    Type: OptionString 
    Required: true
    Name: "title" 
    Description: "Title of the forum thread"
}}

{{$option2 := CmdOption
    Type: OptionString
    Required: true
    Name: "content"
    Description: "Content of the forum thread"
}}

{{addOptions $option1 $option2}}
{% endraw %}
{% endhighlight %}

**command execute code**

{% highlight golang %}
{% raw %}
// Replace with your own forum channel ID
{{$forumChannel := 1152957828013764688}}

{{$post := ForumThread
    Title: (getInput "title").String
    // Content can be a string, embed, or message complex
    //
    // For this example we opt for wrapping input in an embed
    Content: (Embed
        Description: (getInput "content").String
    )
    // Replace with relevant tags for your own forum channel
    Tags: (Array "discussion" "bot")
}}

{{createThread $forumChannel $post}}
{{respondPrivate "Done!"}}
{% endraw %}
{% endhighlight %}

Notice that `Content` can be any of the 3 listed types: string, embed, or message complex. This allows you to create forum threads with unique formatting that normal users aren't able to do.

Also notice that `Tags` accepts a list of forum tag names. These are specific to each forum channel and are case-sensitive (for example, "discussion" is not the same as "Discussion"). Keep in mind that the maximum # of tags per forum thread is 5 which is a Discord limitation.

Here is what it could look like after using the command:

![forum_thread](/assets/t08/forum_thread.png)

Here are all the available ForumThread fields:

.Title | String | **required** | Title of the thread
.AutoArchiveDuration | Int | **optional** | Duration in minutes before Discord auto archives the thread
.Slowmode | Int | **optional** | Duration in seconds where someone must wait before sending a new message
.Content | String, [Embed](/docs#type-embed), or [MessageComplex](/docs#type-messagecomplex) | **required** | Content of the thread
.Tags | Array[String] | **optional** | List of tag names to apply to the thread (max 5 per thread)

# Locking/Unclocking

There is one more set of thread commands: `lockThread` and `unlockThread`. This allows you to use the bot to set a thread's status as locked or unlocked. Once locked, people can't post in the thread anymore unless a moderator unlocks it.

Here are the function signatures:

**lockThread threadID**

**unlockThread threadID**

For both of these, thread ID should be an ID type which you can get from the Discord client itself, or you can pull it out of a [*Channel](/docs#contextchannel) object returned by the bot.

# Note About Archiving

The bot tracks the state for all active threads that it has permission to view. However, Discord has the concept of archiving a thread to prevent the total number of active threads from going over the maximum per guild (1000). When a thread is marked as archived, the bot will forget about it. This is important to know because it can cause calls to functions like `getThread` to fail if you pass in an archived thread ID.

[Next <- Tutorial 9: Functions](/tutorials/t09)