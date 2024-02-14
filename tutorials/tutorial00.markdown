---
permalink: /tutorials/t00
title: Tutorials
layout: page
toc: true
---

This set of tutorials is meant to walk you through some of the core features available with Peaches. It's still a work in progress so you can expect new entries to be added over time.

# Language

The bot uses the [data-driven templating system](https://pkg.go.dev/text/template) available through the Go language. Each piece of code is attached to a type of trigger which determines when the code gets run. A trigger for a slash command "hello" would execute anytime a user of your guild tries to run it from one of the channels it's enabled in, for example. 

The language requires each action to be wrapped in a set of curly braces {% raw %}`{{}}`{% endraw %} to indicate that it is meant to be executed. Variables start with {% raw %}`$`{% endraw %} and are untyped so that they can be assigned any value. When creating a new variable, := is used to create and set the variable simultaneously: {% raw %}`{{$variable := 1234}}`{% endraw %}. Once the variable has been created, you can use = to reassign it a new value: {% raw %}`{{$variable = "new value, new data type"}}`{% endraw %}.

The bot adds a few minor extensions on top of the templating system syntax such as support for `// forward slash comments` and `Label: Value` syntax when creating objects of a given type.

# Features

The bot currently features a range of features including programmable slash commands, various utility triggers for events that happen in your guild (such as user join events), a database for storing information that your code can use and modify later, and support for buttons.

Development on the bot's features will primarily be concentrated on adding new builtin functions, new trigger types (such as user nitro boosted), and other Discord message interaction types such as selection menus.

# Tutorials

* [Tutorial 1: First Slash Command](/peaches-bot.docs/tutorials/t01)
* [Tutorial 2: Data Containers](/peaches-bot.docs/tutorials/t02)
* [Tutorial 3: Slash Command Inputs](/peaches-bot.docs/tutorials/t03)
* [Tutorial 4: Triggers](/peaches-bot.docs/tutorials/t04)
* [Tutorial 5: Embeds](/peaches-bot.docs/tutorials/t05)
* [Tutorial 6: Database](/peaches-bot.docs/tutorials/t06)
* (Under Construction) Tutorial 7: Buttons