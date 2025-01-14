---
permalink: /tutorials/t06
title: Database
layout: page
toc: true
---

This tutorial will walk you through the Peaches database. The database allows you to store pieces of information that your code can reference later in order to help it make decisions about what to do. The data you store is persistent, meaning that even if the bot has to shut down and restart the data will not be deleted.

* TOC
{:toc}

# Database Entry

When you use functions such as `dbGet`, you will either receive nil (no entry present) or a valid database entry with some data for you to use. Here are the fields for each entry:

.CreatedAt | Time | Time the entry was created or last updated
.Key       | String | String key of the entry
.Value     | Any | Actual data of the entry

# Functions

Currently 4 functions are supported (more in the future): `dbGet`, `dbSet`, `dbDel`, and `dbIncr`. Each one is considered a single read or write operation, and per command you can issue 10 of these database calls.

The database itself can store 10 * guild member count entries, and once you exceed that number calls to `dbSet` will fail.

### dbGet id key

Retrieves a single database entry represented by id, key pair. ID should be an int64 type whereas key should be a String type. id does not need to point to any valid Discord ID. So while you might use something like a member ID or a channel ID here, you could also give it any valid number that you want to use.

If no entry was present, this will be nil.

### dbSet id key data

Sets a single database entry represented by id, key pair to be data. ID should be an int64 type whereas key should be a String type. The data you provide will either be converted to a `float64` or a `string`. So integer values will become float64 and time values will become string. Data you retrieve can be converted using `toInt64`, `toFloat64`, `toString` or `toTime`. For example:

{% highlight golang %}
{% raw %}
{{dbSet 0 "time" currentTime}}
{{$db := (dbGet 0 "time").Value}}
{{$time := toTime $db}} // Convert from string -> time
{% endraw %}
{% endhighlight %}

Since int64 gets converted to float64, if you need to deal with something like member IDs, channel IDs, message IDs, etc., you can first call `toString` before storing it in the database, and then `toInt64` after retrieving it from the database.

{% highlight golang %}
{% raw %}
{{dbSet 0 "ChannelID" (toString .Channel.ID)}}
{{$db := (dbGet 0 "ChannelID").Value}}
{{$cid := toInt64 $db}} // Convert from string -> int64
{% endraw %}
{% endhighlight %}

Currently the maximum size for each entry is 8 KiB.

### dbDel id key

Deletes a single database entry represented by id, key pair. ID should be an int64 type whereas key should be a String type.

### dbIncr id key amount

Atomically increments a single database entry represented by id, key pair. amount can be any type that can be converted/cast to an int64. This can be used as a persistent counter to keep track of some amount of elements even if multiple commands are trying to increment/decrement the value at the same time.

This function returns the new value after performing the increment. If the data didn't already exist in the database, a new entry will be created and set to the value provided by amount.

# Example 1: earn and spend

This will create 2 commands: `/earn` which randomly adds some amount to a user's balance, and `/spend` which decrements some amount.

Here is the code for earn:

{% highlight golang %}
{% raw %}
{{$amount := randn 100}}
{{$newBalance := dbIncr context.Member.User.ID "AccountBalance" $amount}}
{{respond (printf "Added %d to your balance. You now have %d points." $amount $newBalance)}}
{% endraw %}
{% endhighlight %}

Here is the initialization code for spend:

{% highlight golang %}
{% raw %}
{{$option := CmdOption
    Type: OptionInteger 
    Required: true
    Name: "amount"
    Description: "Amount to spend"
    MinValue: 1
    MaxValue: 10000
}}
{{addOptions $option}}
{% endraw %}
{% endhighlight %}

And finally here is the code for spend:

{% highlight golang %}
{% raw %}
{{$amount := (index context.Inputs 0).Integer}}
{{$balance := dbGet context.Member.User.ID "AccountBalance"}}

// balance is nil - user hasn't used /earn yet
{{if not $balance}}
    {{respond "You haven't earned any points yet."}}
    {{return}}
{{end}}

// Convert to integer
{{$balance = toInt64 $balance.Value}}

{{if lt $balance $amount}}
    {{respond (printf "You don't have enough points! You have %d points." $balance)}}
    {{return}}
{{end}}

// mult by -1 so that we can invert $amount
{{$newBalance := dbIncr context.Member.User.ID "AccountBalance" (mult $amount -1)}}
{{respond (printf "Your new balance is %d points." $newBalance)}}
{% endraw %}
{% endhighlight %}

# Example 2: number guessing game

This example will create 2 commands: `/new-guess` which will create a new guess if there is no existing guess, and `/guess` which will allow people to input a guess.

Here is the execution code for new-guess:

{% highlight golang %}
{% raw %}
{{$existing := dbGet 0 "RandomNumber"}}
// Guess already exists
{{if $existing}}
    {{respond "You still have to guess the last number I thought of!"}}
    {{return}}
{{end}}

// adding 1 will convert the range from [0, 99] to [1, 100] inclusive
{{$number := (add 1 (randn 100))}}
{{dbSet 0 "RandomNumber" $number}}

{{respond "I'm thinking of a number between 1 and 100..."}}
{% endraw %}
{% endhighlight %}

Here is the initialization code for guess which accepts 1 argument:

{% highlight golang %}
{% raw %}
{{$option := CmdOption
    Type: OptionInteger 
    Required: true
    Name: "guess"
    Description: "Your guess (1-100)"
    MinValue: 1
    MaxValue: 100
}}
{{addOptions $option}}
{% endraw %}
{% endhighlight %}

And finally here is the execution code for guess:

{% highlight golang %}
{% raw %}
{{$existing := dbGet 0 "RandomNumber"}}
// No number yet - ask the user to ask for a new guess
{{if not $existing}}
    {{respond "I haven't thought of a new guess yet. You can use `/new-guess` to start a new game."}}
    {{return}}
{{end}}

// Pull int64 out of entry's value field
{{$number := toInt64 $existing.Value}}
{{$guess := (index context.Inputs 0).Integer}}

{{if eq $guess $number}}
    {{respond (print context.Member " guessed the right answer! The number was " $number ".")}}
    {{dbDel 0 "RandomNumber"}}
{{else if lt $number $guess}}
    {{respond "The number I'm thinking of is less than that guess."}}
{{else}}
    {{respond "The number I'm thinking of is greater than that guess."}}
{{end}}
{% endraw %}
{% endhighlight %}

[Next <- Tutorial 7: Buttons](/tutorials/t07)