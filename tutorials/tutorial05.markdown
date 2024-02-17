---
permalink: /tutorials/t05
title: Embeds
layout: page
toc: true
---

This tutorial will walk you through the process of creating and posting message embeds.

* TOC
{:toc}

# Creating Embeds

An embed is a way to post messages to Discord with advanced formatting options. We can include things like titles, description, author, border color, etc. The bot allows you to reply with an embed using functions like `responsd` or `sendMessage`. Here is an example of creating an embed with a few pieces of information:

{% highlight golang %}
{% raw %}
{{$e := Embed
    Title: "Message Embed Title"
    Description: "Content goes here with a max of 2k characters"
    Color: (hexToInt "#800080")
}}
{{respond $e}}
{% endraw %}
{% endhighlight %}

The builtin function `hexToInt` accepts hex strings of the format "0x....", "#...", or just a string of hex with no leading marker and converts them to int64. Here is what it could look like:

![first_embed](/peaches-bot.docs/assets/t05/first_embed.png)

# Including Author

We can incorporate other pieces of information by using a field like Author which accepts objects of type `EmbedAuthor`. This type accepts a Name and an IconURL, and we can make use of the builtin `.AvatarURL` member function to convert the user's avatar to a valid Discord URL that we can use and scale it up/down accordingly.

{% highlight golang %}
{% raw %}
{{$e := Embed
    Title: "Message Embed Title"
    Description: "Content goes here with a max of 2k characters"
    Color: (hexToInt "#800080")
    Author: (EmbedAuthor 
        Name: (print "Hello, " context.Member.User.Username) 
        IconURL: (context.Member.User.AvatarURL "32") // "32" is a power of two size, can also be empty ""
    )
}}
{{respond $e}}
{% endraw %}
{% endhighlight %}

Here is what it looks like now:

![author](/peaches-bot.docs/assets/t05/author.png)

# Including Thumbnail

Now we can extend the code again to add in the Thumbnail field. This field accepts objects of type `EmbedThumbnail` which allows for a URL, width and height. We can use the same approach we used for Author to get a URL for the thumbnail, only this time a bit larger.

{% highlight golang %}
{% raw %}
{{$e := Embed
    Title: "Message Embed Title"
    Description: "Content goes here with a max of 2k characters"
    Color: (hexToInt "#800080")
    Author: (EmbedAuthor 
        Name: (print "Hello, " context.Member.User.Username) 
        IconURL: (context.Member.User.AvatarURL "32") // "32" is a power of two size, can also be empty ""
    )
    Thumbnail: (EmbedThumbnail
        URL: (context.Member.User.AvatarURL "128")
    )
}}
{{respond $e}}
{% endraw %}
{% endhighlight %}

Here's what it looks like with thumbnail included:

![thumbnail](/peaches-bot.docs/assets/t05/thumbnail.png)

# Other Fields

There are other fields allowed. Here is a table with a full listing:

**type Embed**

.URL | String | **optional** |
.Title | String | **optional** |
.Description | String | **optional** |
.Timestamp | String | **optional** |
.Color | int | **optional** | Integer value of a hex color
.Footer | [EmbedFooter](/peaches-bot.docs/docs#type-embedfooter) | **optional** |
.Image | [EmbedImage](/peaches-bot.docs/docs#type-embedimage) | **optional** |
.Thumbnail | [EmbedThumbnail](/peaches-bot.docs/docs#type-embedthumbnail) | **optional** |
.Video | [EmbedVideo](/peaches-bot.docs/docs#type-embedvideo) | **optional** |
.Provider | [EmbedProvider](/peaches-bot.docs/docs#type-embedprovider) | **optional** |
.Author | [EmbedAuthor](/peaches-bot.docs/docs#type-embedauthor) | **optional** |
.Fields | Array[[EmbedField](/peaches-bot.docs/docs#type-embedfield)] | **optional** |

[Next: Tutorial 6: Database](/peaches-bot.docs/tutorials/t06)