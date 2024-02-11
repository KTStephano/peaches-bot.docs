---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
title: Home

---

# Overview

Peaches is a programmable slash command and trigger bot. If you have created custom commands using YAGPDB, this will be a very familiar experience as it follows a similar style.

The bot is currently in the early stages of development so some bugs are expected. They will be fixed as quickly as possible. Please don't hesitate to join the Discord Support Server with any questions you have or to report any issues you encounter.

# Inviting Peaches

You can invite Peaches using [this invite link](https://discord.com/api/oauth2/authorize?client_id=1201100920189096018&permissions=1102196361408&scope=bot%20applications.commands).

# Language

Peaches uses the text templating system offered by the Go programming language. It allows for a mix of static text and dynamic actions wrapped in {% raw %}`{{}}`{% endraw %} tags (action tags).

When a new slash command is created, it is represented internally as an empty text template. If called it will produce the default output.

To program a slash command after it is created, you can upload your own text template code for the bot to use instead.

# Conventions

Variables are prefixed by $. Creating a new variable is done with :=, whereas assigning to an existing variable is done with =.

{% highlight golang %}
{% raw %}
{{$name := "New variable created with :="}}
{{$name = "Existing value being overwritten with ="}}
{% endraw %}
{% endhighlight %}

Types, type functions, fields and constants are represented by words that start with capital letters. Examples of types are `MessageEmbed` and `MessageComplex` whereas examples of constants are `E`, `Pi`, `Phi`.

When creating an object of a certain type, you can set its fields using `FieldName: Value` syntax. The following example would create a new MessageEmbed object and store it in the embed variable.

{% highlight golang %}
{% raw %}
{{$embed := MessageEmbed
    Title: "Title Goes Here"
    Description: "Description text goes here."
}}
{% endraw %}
{% endhighlight %}

Regular functions start with lowercase letters such as `len`, `add`, `index`.