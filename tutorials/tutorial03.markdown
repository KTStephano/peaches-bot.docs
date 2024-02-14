---
permalink: /tutorials/t03
title: Slash Command Inputs
layout: page
toc: true
---

This tutorial will walk you through the customization of slash commands to accept both required and optional inputs.

* TOC
{:toc}

# Custom command initialization

In the first tutorial we glanced over the first source code text box that appears with `/create-cmd`. This allows you to specify some code that runs during the command creation stage and tells the bot more about the layout of the command. This layout includes not only the inputs but also the default permissions.

Command initialization exposes a few functions and data types to use. The functions are `addOptions` and `setDefaultPermissions` whereas the data types are `OptionType`, `ChannelType`, `CmdOption` and `CmdChoice` which work together.

Here is the layout for the data types:

**enum OptionType**

Can be one of OptionString, OptionInteger, OptionBoolean, OptionUser, OptionChannel, OptionRole, OptionMentionable, OptionNumber.

**enum ChannelType**

Can be one of ChannelGuildText, ChannelDM, ChannelGuildVoice, ChannelGuildGroupDM, ChannelGuildCategory, ChannelGuildNews, ChannelGuildStore, ChannelGuildNewsThread, ChannelGuildPublicThread, ChannelGuildPrivateThread, ChannelGuildStageVoice, ChannelGuildForum.

**type CmdOption**

.Type | [OptionType](/peaches-bot.docs/docs#enum-optiontype) | **required** | Type of data this option can contain. This determines what inputs the Discord UI will accept. 
.Name | String | **required** | Top-level name of the option
.Description | String | **required** | Short explanation of the option
.ChannelTypes | Array[[ChannelType](/peaches-bot.docs/docs#enum-channeltype)] | **optional** | An array of channel types that this option accepts
.Required | Bool | **optional** (default false) | Whether this option is required or not
.Choices | Array[OptionType] | **optional** | Selection menu that the user can choose from
.MinValue | Double | **optional** | Minimal value of number/integer option
.MaxValue | Double | **optional** | Maximum value of number/integer option
.MinLength | Int64 | **optional** | Minimum length of string option
.MaxLength | Int64 | **optional** | Maximum length of string option

**type CmdChoice**

**Fields**

.Name | String | **required** | Name that appears in the slash command selection menu
.Value | Any | **required** | Value assigned to the choice for use by the code

In the next section we will make use of these types to customize a slash command's inputs.

# Example 1: Ping User

For the first example we will create a command called "hello" that takes a required user input argument. It responds by saying "Hello, @user!". To do this you can use the `/create-cmd` command again to bring up the editor. Once there, in the first bo we will add an argument of type `OptionUser` with name "user", description "User to ping", and required set to true.

![ping](/peaches-bot.docs/assets/t03/ping.png)

Notice that inside of CmdOption there are the `Type:`, `Required:`, `Name:` and `Description:` labels followed by their values. This is how to create new objects of a given type with the bot's scripting language.

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
{{addOption $option}}
{% endraw %}
{% endhighlight %}

This tells the bot to register the given option for the command. This piece of code is executed only once during the creation of the slash command. After that, the bot doesn't use it again even if you call `/set-source` later.

When trying to run the command in Discord, this is what it might look like with suggestions.

![ping_input](/peaches-bot.docs/assets/t03/ping_input.png)

Now walking through the command execution part of the code:

{% highlight golang %}
{% raw %}
{{$user := (index context.Inputs 0).User}}
{{respond (printf "Hello, %s!" $user.Mention)}}
{% endraw %}
{% endhighlight %}

The main thing to know here is that context.Inputs is populated by the bot with a list of command inputs from the user. The `index` function can be used to index any array (even ones passed in from the bot's system) with the array as the first argument and index as the second.

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
{{$user := (index context.Inputs 0).User}}
// If length > 1, we know we got a message input as arg #2
{{if gt (len context.Inputs) 1}}
    {{$message := (index context.Inputs 1).String}}
    {{respond (printf "%s: %s" $user.Mention $message)}}

// Else we only have the user argument - print the default message
{{else}}
    {{respond (printf "Hello, %s!" $user.Mention)}}
{{end}}
{% endraw %}
{% endhighlight %}

# Example 3: Let user select a role

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
{{$roleIdList := Array
    1206492832936886332 // green - replace with your own role id
    1206492862775304222 // blue - replace with your own role id
}}

// Selection given to us by the user, as an integer
{{$selection := (index context.Inputs 0).Integer}}
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

After a command has been created, it's possible to manage its permissions from within Discord itself. To do this, in your server go to `Server Settings`, then `Integrations`, then find Peaches. From here you can click on the commands and edit the permissions. Here is an example of editing the hello command from the first tutorial:

![permissions](/peaches-bot.docs/assets/t03/permissions.png)

If saved, this would have the effect of disabling it for @everyone but enabling it specifically for people with the @Level 25 role. It also makes it so that it is completely disabled from the #role-selection channel.

[Next: Tutorial 4: Triggers](/peaches-bot.docs/tutorials/t04)