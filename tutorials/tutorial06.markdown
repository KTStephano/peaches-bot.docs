---
permalink: /tutorials/t06
title: Database
layout: page
toc: true
---

This tutorial will walk you through the Peaches database.

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

Sets a single database entry represented by id, key pair to be data. ID should be an int64 type whereas key should be a String type. The data itself can be anything that can be converted to JSON format. This includes ints and floats (ints are converted to float by default when storing them), but also strings, maps and arrays.

Currently the maximum size for each entry is 8 KiB.

### dbDel id key

Deletes a single database entry represented by id, key pair. ID should be an int64 type whereas key should be a String type.

### dbIncr id key amount

Atomically increments a single database entry represented by id, key pair. amount can be any type that can be converted/cast to an int64. This can be used as a persistent counter to keep track of some amount of elements even if multiple commands are trying to increment/decrement the value at the same time.

This function returns the new value after performing the increment. If the data didn't already exist in the database, a new entry will be created and set to the value provided by amount.

# Example: earn and spend

This will create 2 functions: earn which randomly adds some amount to a user's balance, and spend which decrements some amount.

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