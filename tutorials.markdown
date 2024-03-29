---
title: Tutorials
layout: page
permalink: /tutorials
---

This set of tutorials is meant to walk you through some of the core features available with the Peaches scripting system. It's still a work in progress so you can expect new entries to be added over time.

# Scripting Language

Peaches uses the simple [text templating system](https://pkg.go.dev/text/template) available through the Go language. Each piece of code is attached either to a slash command or guild trigger which determines when it gets run. For example, a script attached to `/hello` will run anytime a user tries to use your `/hello` slash command in a channel.

The language requires each action to be wrapped in a set of curly braces {% raw %}`{{}}`{% endraw %} to indicate that the line of code meant to be executed. Variables start with {% raw %}`$`{% endraw %} and are untyped so that they can be assigned any value. When creating a new variable, := is used to create and set the variable simultaneously: {% raw %}`{{$variable := 1234}}`{% endraw %}. Once the variable has been created, you can use = to reassign it a new value: {% raw %}`{{$variable = "new value, new data type"}}`{% endraw %}.

The bot adds a few minor extensions on top of the templating system syntax such as support for `// forward slash comments` and `Label: Value` syntax when creating objects of a given type. Examples of both of these can be found in one or more of the tutorials listed below.

# Scripting Features

The bot includes a range of features including programmable slash commands, various utility triggers for events that happen in your guild (such as user join events), a database for storing information that your code can use and modify later, and support for buttons.

Development of the bot's scripting language features will primarily be concentrated on adding new builtin functions, new trigger types, and other Discord message interaction types such as selection menus.

# Tutorials

* [Tutorial 1: First Slash Command](/tutorials/t01)
* [Tutorial 2: Slash Command Inputs](/tutorials/t02)
* [Tutorial 3: Data Containers](/tutorials/t03)
* [Tutorial 4: Triggers](/tutorials/t04)
* [Tutorial 5: Embeds](/tutorials/t05)
* [Tutorial 6: Database](/tutorials/t06)
* [Tutorial 7: Buttons](/tutorials/t07)
* [Tutorial 8: Threads](/tutorials/t08)
* [Tutorial 9: Functions](/tutorials/t09)
* [Tutorial 10: Roles](/tutorials/t10)