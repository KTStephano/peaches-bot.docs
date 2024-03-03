---
permalink: /tutorials/t10
title: Roles
layout: page
toc: true
---

**Note** Make sure the bot has permission to manage your server's roles.

This tutorial will walk you through some of the more advanced role management functions. In previous tutorials we made use of `hasRole`, `giveRole`, `takeRole`, but this tutorial will focus on creating, editing and deleting roles. It will showcase an example that gives your server admins the ability to allow certain users to create their own role and then freely change its name and color.

* TOC
{:toc}

# Create

In order to create a new role, you have to make use of the `RoleInfo` type. This type allows you to specify some information about the new role that you're creating so that the bot can forward it to Discord.

Here are the available pieces of data that `RoleInfo` can accept:

.Name | String | **required** | The role's name
.Color | Integer | **optional** | The color the role should have (as a decimal, not hex)
.Separate | Bool | **optional** | If true, members with this role show up separately on the side bar
.Permissions | Array[[DiscordPermission](/docs/#bitfield-discordpermission)] | **optional** | The overall permissions of the role
.Mentionable | Bool | **optional** | Whether this role is mentionable

It's possible to create a new role by only supplying the name. This will result in a new role with all the settings set to default.

Once you've set up your `RoleInfo` data, you can pass it along to `createRole`. Here is an example:

{% highlight golang %}
{% raw %}
// Create a role with default settings
{{$role := RoleInfo
    Name: "My Custom Role"
}}

{{$role = createRole $role}}
// Check if we got a valid role back
{{if $role}}
    {{respond (print "Created role: " $role.Name " " $role.ID)}}
{{else}}
    {{respond "Failed to create role :("}}
{{end}}
{% endraw %}
{% endhighlight %}

Here is another example, but this time we specify a color:

{% highlight golang %}
{% raw %}
{{$role := RoleInfo
    Name: "My Custom Role"
    Color: (hexToInt "#ab65cf")
}}

{{$role = createRole $role}}
{% endraw %}
{% endhighlight %}

With this example it sets a couple of the permissions:

{% highlight golang %}
{% raw %}
{{$role := RoleInfo
    Name: "My Custom Role"
    Permissions: (Array PermissionSendMessages PermissionSendMessagesInThreads)
}}

{{$role = createRole $role}}
{% endraw %}
{% endhighlight %}

# Editing

For any role that the bot can manage, there are a few other functions that can edit these existing roles:

**editRoleName roleID newName**

This sets the role given by `roleID` to have `newName`. `roleID` is the Discord ID, not the role's name.

**editRoleColor roleID color**

This sets the role given by `roleID` to have `color` which should be a decimal (not hex). `roleID` is the Discord ID, not the role's name.

**moveRoleAfter roleID afterID**

**moveRoleBefore roleID beforeID**

These two move functions deal with the Discord role hierarchy. In Discord, roles that come after a certain role are higher up in the hierarchy, while roles that come before a certain role are lower down in the hierarchy. This can have a big impact on what permissions a role has: for example, trying to edit a role higher than your own highest role is not allowed.

The bot is able to shuffle roles around using `moveRoleAfter` and `moveRoleBefore`. The catch is that the bot is not allowed to move a role to a position higher than its own highest role. You will need to go into your guild's settings and make sure the bot's highest role sits somewhere in the role hierarchy such that allows it to do everything you need the bot to do.

Also keep in mind that `roleID`, `afterID`, and `beforeID` all need to reference the role IDs provided by Discord. They are not the role names.

# Deleting

You can also delete a role using `deleteRole roleID`. For this function, `roleID` is still the Discord ID, not the role's name.

# Example: Custom role for a member

This example will create a few functions. The first two are functions that only your guild administrators can use, and they enable them to allow and disallow server members to create and manage their own role.

### allow-role

`/create-cmd name: allow-role description: Allow a certain user to create and manage their own role`

**Command initialization code**

{% highlight golang %}
{% raw %}
{{$option := CmdOption
    Type: OptionUser 
    Required: true
    Name: "user" 
    Description: "User to allow"
}}

{{addOptions $option}}

// Administrators only
{{setDefaultPermissions PermissionAdministrator}}
{% endraw %}
{% endhighlight %}

**Command execute code**

{% highlight golang %}
{% raw %}
{{$user := (getInput "user").User}}
{{dbSet $user.ID "CustomRoleAllowed" true}}
{{respondPrivate (print $user.Username " can now create their own role")}}
{% endraw %}
{% endhighlight %}

### disallow-role

`/create-cmd name: disallow-role description: Disallows a user from having a custom role`

**Command initialization code**

{% highlight golang %}
{% raw %}
{{$option := CmdOption
    Type: OptionUser 
    Required: true
    Name: "user" 
    Description: "User to allow"
}}

{{addOptions $option}}

// Administrators only
{{setDefaultPermissions PermissionAdministrator}}
{% endraw %}
{% endhighlight %}

**Command execution code**

{% highlight golang %}
{% raw %}
{{$user := (getInput "user").User}}
{{$allowed := dbGet $user.ID "CustomRoleAllowed"}}

{{if not $allowed}}
    {{respond "That user was not allowed to have a custom role"}}
    {{return}}
{{end}}

{{$role := dbGet $user.ID "CustomRoleID"}}

{{if $role}}
    {{$role = toInt64 $role.Value}}
    {{deleteRole $role}}
{{end}}

// Clear out database entries
{{dbDel $user.ID "CustomRoleAllowed"}}
{{dbDel $user.ID "CustomRoleID"}}

{{respond (print $user.Username " can no longer create a custom role.")}}
{% endraw %}
{% endhighlight %}

The next functions will be all about creating a custom role, editing its name, and editing its color.

### claim-role

`/create-cmd name: claim-role description: Claim a custom role in the server`

**Command initialization code**

{% highlight golang %}
{% raw %}
{{$option1 := CmdOption
    Type: OptionString 
    Required: true
    Name: "name" 
    Description: "Name of your custom role"
}}

{{$option2 := CmdOption
    Type: OptionInteger 
    Required: true
    Name: "color" 
    Description: "Color of your custom role as a decimal"
    MinValue: 0
    MaxValue: 16777215
}}

{{addOptions $option1 $option2}}
{% endraw %}
{% endhighlight %}

**Command execution code**

{% highlight golang %}
{% raw %}
{{$user := context.Member.User}}
{{$allowed := dbGet $user.ID "CustomRoleAllowed"}}

{{if not $allowed}}
    {{respondPrivate "Sorry but you are not allowed to create a custom role."}}
    {{return}}
{{end}}

{{$roleID := dbGet $user.ID "CustomRoleID"}}

{{if $roleID}}
    {{$roleID = toInt64 $roleID.Value}}

    // Check if the role still exists
    {{if (getRole $roleID)}}
        {{giveRole $user.ID $roleID}}
        {{respondPrivate "You already have a custom role! Use role-edit to change it."}}
        {{return}}
    {{end}}
{{end}}

// Create a new role
{{$info := RoleInfo
    Name: (getInput "name").String
    Color: (getInput "color").Integer
}}

{{$role := createRole $info}}

// Make sure it worked
{{if not $role}}
    {{respond "Something went wrong. :( Try again later."}}
    {{return}}
{{end}}

{{respond (print "Created a custom role named " $info.Name)}}

// Update database and give user the new role
{{dbSet $user.ID "CustomRoleID" (toString $role.ID)}}
{{giveRole $user.ID $role.ID}}
{% endraw %}
{% endhighlight %}

### edit-role

`/create-cmd name: edit-role description: Edit your custom role`

**Command initialization code**

{% highlight golang %}
{% raw %}
{{$option1 := CmdOption
    Type: OptionString 
    Required: false
    Name: "name" 
    Description: "New name of your custom role"
}}

{{$option2 := CmdOption
    Type: OptionInteger 
    Required: false
    Name: "color" 
    Description: "New color of your custom role as a decimal"
    MinValue: 0
    MaxValue: 16777215
}}

{{addOptions $option1 $option2}}
{% endraw %}
{% endhighlight %}

**Command execution code**

{% highlight golang %}
{% raw %}
{{$user := context.Member.User}}
{{$roleID := dbGet $user.ID "CustomRoleID"}}

{{if not $roleID}}
    {{respondPrivate "You do not have a custom role. Use `/claim-role` to make one."}}
    {{return}}
{{end}}

{{$roleID = toInt64 $roleID.Value}}

{{$name := getInput "name"}}
{{$color := getInput "color"}}

// Both name and color are optional inputs so we need to check
// to see if the user gave them to us
{{if $name}}
    {{editRoleName $roleID $name.String}}
{{end}}

{{if $color}}
    {{editRoleColor $roleID $color.Integer}}
{{end}}

{{respond "Finished editing your role!"}}
{% endraw %}
{% endhighlight %}

# Limitations

Currently there is no way to set and manage a role image through the bot. This is a feature that is planned but may take some time to implement.