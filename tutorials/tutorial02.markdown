---
permalink: /tutorials/t02
title: Slash Command Inputs
layout: page
toc: true
---

This tutorial will walk you through the customization of slash commands to accept both required and optional inputs.

* TOC
{:toc}

# Custom command initialization

In the first tutorial we glanced over the first source code text box that appears with `/create-cmd`. This allows you to specify some code that runs during the command creation stage and tells the bot more about the layout of the command. This layout includes not only the inputs but also the default permissions.

Command initialization exposes a few functions and data types to use. The functions are `addOptions` and `setDefaultPermissions`. Together these functions let you customize the command inputs (options) that a user can give to it, as well as the default required permissions that a member needs to have in order to use the command. Command permissions can be modified later through the Discord client.

Here is the layout for the input types that options can accept:

**enum OptionType**

Can be one of OptionString, OptionInteger, OptionBoolean, OptionUser, OptionChannel, OptionRole, OptionMentionable, OptionNumber.

**enum ChannelType**

Can be one of ChannelGuildText, ChannelDM, ChannelGuildVoice, ChannelGuildGroupDM, ChannelGuildCategory, ChannelGuildNews, ChannelGuildStore, ChannelGuildNewsThread, ChannelGuildPublicThread, ChannelGuildPrivateThread, ChannelGuildStageVoice, ChannelGuildForum.

# Example 1: Ping User

For the first example we will create a command called "hello" that takes a required user input argument. It responds by saying "Hello, @user!". To do this you can use the `/create-cmd` command again to bring up the editor. Once there, in the first box we will add an argument of type `OptionUser` with name "user", description "User to ping", and required set to true.

![ping](/peaches-bot.docs/assets/t03/ping.png)

Notice that inside of CmdOption there are the `Type:`, `Required:`, `Name:` and `Description:` labels followed by their values. This is how to create new objects of a given type with the bot's scripting language. We are also using `printf` which stands for `print formatted`. It takes a format string as its first argument that can have various text placeholders such as {% raw %}`%d`{% endraw %} (decimal number) or {% raw %}`%s`{% endraw %} (string).

Walking through the source code in the first text box:

{% highlight golang %}
{% raw %}
{{$option := CmdOption
    Type: OptionUser Required: true
    Name: "user" Description: "User to ping"}}
{% endraw %}
{% endhighlight %}

This creates a new command object object which is of type `OptionUser`. This tells Discord to only allow valid users as inputs, and it will prevent any values outside of that from being entered. In the slash command menu it will contain the name "user" and description "User to ping".

{% highlight golang %}
{% raw %}
{{addOptions $option}}
{% endraw %}
{% endhighlight %}

This tells the bot to register the given option for the command. This piece of code is executed only once during the creation of the slash command. After that, the bot doesn't use it again even if you call `/set-source` later.

When trying to run the command in Discord, this is what it might look like with suggestions.

![ping_input](/peaches-bot.docs/assets/t03/ping_input.png)

Now walking through the command execution part of the code:

{% highlight golang %}
{% raw %}
{{$user := (getInput 0).Value}}
{{respond (printf "Hello, %s!" $user)}}
{% endraw %}
{% endhighlight %}

Here we use the `getInput` function and pass it in 0. `getInput` is a builtin convenience function that provides the ability to either get an input by its index (0 in this case, which is the first index), or by its option name (such as "user"). `.Value` at the end gets the value of the input, whether it be an integer, boolean, member, etc.

# Example 2: Ping with optional message

Now we'll extend the original command by running `/create-cmd` again, but this time adding a second optional input to act as the custom message.

Command customization code:

{% highlight golang %}
{% raw %}
{{$option1 := CmdOption
    Type: OptionUser 
    Required: true
    Name: "user" 
    Description: "User to ping"
}}
{{$option2 := CmdOption
    Type: OptionString 
    Required: false
    Name: "message"
    Description: "Custom message to include"
}}
{{addOptions $option1 $option2}}
{% endraw %}
{% endhighlight %}

This will re-create the command with 2 inputs, the second with `Required` set to false.

Inside of the command execution code, we know that the first argument will always be present since it is set to required, but the second we have to optionally check for. We can do something like this:

{% highlight golang %}
{% raw %}
{{$user := (getInput 0).Value}}
// If length of Inputs > 1, we know we got a message input as arg #2
{{if gt (len context.Inputs) 1}}
    {{$message := (getInput 1).Value}}
    {{respond (printf "%s: %s" $user $message)}}

// Else we only have the user argument - print the default message
{{else}}
    {{respond (printf "Hello, %s!" $user)}}
{{end}}
{% endraw %}
{% endhighlight %}

`context.Inputs` in the above code is where the bot puts all of the inputs for the command. The `context` is all the information that your code has about who used the command, what inputs they gave it, what channel they ran it in, etc.

# Handling multiple optional inputs

Some commands you create could end up having multiple optional inputs. This leads to a situation where you need to have a good way of checking for an input being present and ignoring it if not. For this you can use the `getInput` function but instead of giving it an input index, you can give it an input name.

**getInput arg**

`arg` can either be the an index into context.Inputs or it can be a string representing a command option's name.

If the input doesn't exist it will return empty (nil).

# Example 3: Ping with optional message and channel

This example will show the benefit of using `getInput` for allowing you to effectively handle multiple optional inputs.

Command customization code:

{% highlight golang %}
{% raw %}
{{$option1 := CmdOption
    Type: OptionUser 
    Required: true
    Name: "user" // this is the name passed to getInput
    Description: "User to ping"
}}

{{$option2 := CmdOption
    Type: OptionString 
    Required: false
    Name: "message" // this is the name passed to getInput
    Description: "Custom message to include"
}}

{{$option3 := CmdOption
    Type: OptionChannel 
    Required: false
    Name: "channel" // this is the name passed to getInput
    Description: "Channel to send the message to"
}}

{{addOptions $option1 $option2 $option3}}
{% endraw %}
{% endhighlight %}

Here is the new command execution code:

{% highlight golang %}
{% raw %}
// Since "user" is a required argument, we don't need to error check
{{$user := (getInput "user").Value}}

// These will need to be checked for empty (nil)
{{$message := getInput "message"}}
{{$channel := getInput "channel"}}

{{if $channel}}
    // Use input channel
    {{$channel = $channel.Value.ID}}
{{else}}
    // Use context channel
    // (tells us which channel this command is executing in)
    {{$channel = context.Channel.ID}}
{{end}}

{{if $message}}
    // Use custom message
    {{$message = printf "%s: %s" $user $message.Value}}
{{else}}
    // Use default message
    {{$message = printf "Hello, %s!" $user}}
{{end}}

// Use sendMessage so we can specify the channel
{{sendMessage $channel $message}}

// Still need to respond so Discord doesn't think we ignored the user
{{respondPrivate "Message sent!"}}
{% endraw %}
{% endhighlight %}

# Example 4: Let user select a role

This example will demonstrate how to use `CmdChoice` in order to create a list of optional roles that the user can select from. We'll do this by giving them a set of options that internally map to an array of role ids.

{% highlight golang %}
{% raw %}
{{$role1 := CmdChoice
    Name: "green"
    Value: 0 // index of role id
}}
{{$role2 := CmdChoice
    Name: "blue"
    Value: 1 // index of role id
}}
{{$option := CmdOption
    Type: OptionInteger 
    Name: "role-options"
    Description: "List of roles you can select from"
    Required: true
    Choices: (Array $role1 $role2)
}}
{{addOptions $option}}
{% endraw %}
{% endhighlight %}

For the source code, we will have an array where each entry corresponds to an index contained in the selection.

{% highlight golang %}
{% raw %}
// Array is covered in the next tutorial - it holds a list of items
{{$roleIdList := Array
    1206492832936886332 // green - replace with your own role id
    1206492862775304222 // blue - replace with your own role id
}}

// Selection given to us by the user, as an integer
{{$selection := (getInput "role-options").Integer}}
{{$roleId := index $roleIdList $selection}}
{{$userId := context.Member.User.ID}}

// If user has the role, let them know
{{if (hasRole $userId $roleId)}}
    {{respond "You already have that role!"}}

{{else}}
    {{giveRole $userId $roleId}}
    {{respond "Gave you the role"}}
{{end}}
{% endraw %}
{% endhighlight %}

This is what the user will see when they try running the command:

![select](/peaches-bot.docs/assets/t03/select.png)

# Modifying slash command permissions

After a command has been created, it's possible to manage its permissions from within Discord itself. To do this, in your server go to `Server Settings`, then `Integrations`, then find `Peaches`. From here you can click on the commands and edit the permissions. Here is an example of editing the hello command from the first tutorial:

![permissions](/peaches-bot.docs/assets/t03/permissions.png)

If saved, this would have the effect of disabling it for @everyone but enabling it specifically for people with the @Level 25 role. It also makes it so that it is completely disabled from the #role-selection channel.

[Next <- Tutorial 3: Data Containers](/peaches-bot.docs/tutorials/t03)