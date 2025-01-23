---
permalink: /docs
title: Documentation
layout: page
toc: true
---

# Table of Contents
* TOC
{:toc}

# Programmable Stages

There are two stages that can be programmed. 

The first is the **command creation** stage. This will only be run when the command is first being created. It allows you to write code that sets the command's various input options and their choices as well as its default permissions. If you later need to change the command setup (name, description, options), you will need to re-run this step. Changing permissions can be done for any command from inside of Server Settings > Integrations > Peaches.

The second is the **trigger execution** stage. There are multiple types of triggers, one of them being a slash command being entered by a user.


# Math Constants

### MaxInt
### MinInt
### MaxInt64
### MinInt64
### MaxFloat
### MaxDouble
### Pi
### E
### SqrtE
### Phi
### SqrtPhi
### Nanosecond
### Microsecond
### Millisecond
### Second
### Minute
### Hour

# Context

When the **trigger execution** stage is run, it is initialized with a context that informs the code about what triggered it. This can include information about the guild, member, channel, etc. It can be retrieved through the `context` builtin variable.

### context.Guild

**Fields**

.ID | The ID of the guild
.Name | The name of the guild (2–100 characters)
.Icon | The hash of the guild's icon
.Region | The voice region of the guild
.AfkChannelID | The ID of the AFK voice channel
.OwnerID | The user ID of the owner of the guild
.DiscoverySplash | The hash of the guild's discovery splash
.Splash | The hash of the guild's splash
.AfkTimeout | The timeout, in seconds, before a user is considered AFK in voice
.MemberCount | The number of members in the guild
.Features | The list of enabled guild features
.WidgetEnabled | Whether or not the Server Widget is enabled
.WidgetChannelID | The Channel ID for the Server Widget
.SystemChannelID | The Channel ID to which system messages are sent (eg join and leave messages)
.SystemChannelFlags | The System channel flags
.RulesChannelID | The ID of the rules channel ID, used for rules
.VanityURLCode | the vanity url code for the guild
.Description | the description for the guild
.Banner | The hash of the guild's banner
.PremiumTier | The premium tier of the guild
.PremiumSubscriptionCount | The total number of users currently boosting this server
.PreferredLocale | The preferred locale of a guild with the "PUBLIC" feature; used in server discovery and notices from Discord; defaults to "en-US"
.PublicUpdatesChannelID | The id of the channel where admins and moderators of guilds with the "PUBLIC" feature receive notices from Discord

### context.Member

**May be nil if a member did not trigger the code**

.GuildID | The guild ID on which the member exists
.JoinedAt | The time at which the member joined the guild
.Nick | The nickname of the member, if they have one
.Avatar | The hash of the avatar for the guild member, if any
.User | The underlying user on which the member is based
.Roles | A list of IDs of the roles which are possessed by the member
.PremiumSince | When the user used their Nitro boost on the server
.Pending | Is true while the member hasn't accepted the membership screen
.Permissions | Total permissions of the member in the channel, including overrides, returned when in the interaction object
.CommunicationDisabledUntil | The time at which the member's timeout will expire (time in the past or nil if the user is not timed out)

### context.Member.User

**May be nil if a member did not trigger the code**

.ID | The ID of the user
.Username | The user's username
.Avatar | The hash of the user's avatar.
.Locale | The user's chosen language option
.GlobalName | The user's display name, if it is set (for bots, this is the application name)
.Verified | Whether the user's email is verified
.MFAEnabled | Whether the user has multi-factor authentication enabled
.Banner | The hash of the user's banner image
.AccentColor | User's banner color, encoded as an integer representation of hexadecimal color code
.Bot | Whether the user is a bot
.PublicFlags | The public flags on a user's account
.PremiumType | The type of Nitro subscription on a user's account
.System | Whether the user is an Official Discord System user (part of the urgent message system)
.Flags | The flags on a user's account

### context.Channel

**May be nil if a channel was not involved in triggering the code**

.ID | The ID of the channel
.GuildID | The ID of the guild
.Name | The name of the channel
.Topic | The topic of the channel
.IsThread | True if channel represents an individual thread
.IsForum | True if channel represents a Forum channel
.Type | The type of the channel
.LastMessageID | The ID of the last message sent in the channel (may be invalid)
.LastPinTimestamp | he timestamp of the last pinned message in the channel (nil if no pinned messages)
.MessageCount | An approximate count of messages in a thread, stops counting at 50
.MemberCount | An approximate count of users in a thread, stops counting at 50
.NSFW | Whether the channel is marked as NSFW
.Icon | Icon of the group DM channel
.Position | The position of the channel, used for sorting in client
.Bitrate | The bitrate of the channel, if it is a voice channel
.PermissionOverwrites | A list of permission overwrites present for the channel
.UserLimit | The user limit of the voice channel
.ParentID | The ID of the parent channel
.RateLimitPerUser | Amount of seconds a user has to wait before sending another message or creating another thread (0-21600)
.OwnerID | ID of the creator of the group DM or thread
.ApplicationID | ApplicationID of the DM creator Zeroed if guild channel or not a bot user
.ThreadMetadata | Thread-specific fields not needed by other channels
.Flags | Channel flags
.AvailableTags | The set of tags that can be used in a forum channel
.AppliedTags | The IDs of the set of tags that have been applied to a thread in a forum channel
.DefaultReactionEmoji | Emoji to use as the default reaction to a forum post
.DefaultThreadRateLimitPerUser | The initial RateLimitPerUser to set on newly created threads in a channel
.DefaultSortOrder | The default sort order type used to order posts in forum channels
.DefaultForumLayout | The default forum layout view used to display posts in forum channels

### context.Message

**May be nil if a message did not trigger the code**

.ID | The ID of the message
.ChannelID | The ID of the channel in which the message was sent
.GuildID | The ID of the guild in which the message was sent
.Content | The content of the message
.ContentWithMentionsReplaced | The content of the message but with @uid replaced with the username
.Timestamp | The time at which the messsage was sent
.EditedTimestamp | The time at which the last edit of the message
.MentionRoles | The roles mentioned in the message
.TTS | Whether the message is text-to-speech
.MentionEveryone | Whether the message mentions everyone
.Author | The author of the message. This is not guaranteed to be a valid user (webhook-sent messages do not possess a full author).
.Attachments | A list of attachments present in the message
.Components | A list of components attached to the message
.Embeds | A list of embeds present in the message
.Mentions | A list of users mentioned in the message
.Reactions | A list of reactions to the message
.Pinned | Whether the message is pinned or not
.Type | The type of the message
.WebhookID | The webhook ID of the message, if it was generated by a webhook
.Member | Member properties for this message's author (contains only partial information)
.MentionChannels | Channels specifically mentioned in this message
.ReferencedMessage | The message associated with the message_reference. NOTE: This field is only returned for messages with a type of 19 (REPLY) or 21 (THREAD_STARTER_MESSAGE).
.Flags | The flags of the message, which describe extra features of a message
.Thread | The thread that was started from this message, includes thread member object
.StickerItems | An array of Sticker objects, if any were sent

### context.Inputs

An array of inputs to a Slash Command. Type is [*SlashCommandInputData](/docs#type-slashcommandinputdata).

### context.AuditEntry

**Only valid for triggers involving audit log entries**

An [*AuditLogTriggerData](/docs#type-auditlogtriggerdata) object.

### context.Interaction

**Only valid for triggers involving webhook interactions, such as buttons**

A [*MessageComponentInteractionData](/docs#type-messagecomponentinteractiondata) object.

# Types

### enum OptionType

Can be one of OptionString, OptionInteger, OptionBoolean, OptionUser, OptionChannel, OptionRole, OptionMentionable, OptionNumber.

### enum ChannelType

Can be one of ChannelGuildText, ChannelDM, ChannelGuildVoice, ChannelGuildGroupDM, ChannelGuildCategory, ChannelGuildNews, ChannelGuildStore, ChannelGuildNewsThread, ChannelGuildPublicThread, ChannelGuildPrivateThread, ChannelGuildStageVoice, ChannelGuildForum.

### bitfield DiscordPermission

Can be one of PermissionCreateInstantInvite, PermissionKickMembers, PermissionBanMembers, PermissionAdministrator, PermissionManageChannels, PermissionManageGuild, PermissionAddReactions, PermissionViewAuditLog, PermissionPrioritySpeaker, PermissionStream, PermissionViewChannel, PermissionSendMessages, PermissionSendTTSMessages, PermissionManageMessages, PermissionEmbedLinks, PermissionAttachFiles, PermissionReadMessageHistory, PermissionMentionEveryone, PermissionUseExternalEmojis, PermissionViewGuildInsights, PermissionConnect, PermissionSpeak, PermissionMuteMembers, PermissionDeafenMembers, PermissionMoveMembers, PermissionUseVAD, PermissionChangeNickname, PermissionManageNicknames, PermissionManageRoles, PermissionManageWebhooks, PermissionManageGuildExpressions, PermissionUseApplicationCommands, PermissionRequestToSpeak, PermissionManageEvents, PermissionManageThreads, PermissionCreatePublicThreads, PermissionCreatePrivateThreads, PermissionUseExternalStickers, PermissionSendMessagesInThreads, PermissionUseEmbeddedActivities, PermissionTimeoutMembers, PermissionViewCreatorMonetizationAnalytics, PermissionUseSoundboard, PermissionCreateGuildExpressions, PermissionCreateEvents, PermissionUseExternalSounds, PermissionSendVoiceMessages.

### enum ButtonStyle

Can be one of PrimaryButton, SecondaryButton, SuccessButton, DangerButton, LinkButton.

### enum ComponentType

Can be one of ActionsRowComponent, ButtonComponent, SelectMenuComponent, TextInputComponent, UserSelectMenuComponent, RoleSelectMenuComponent, MentionableSelectMenuComponent, ChannelSelectMenuComponent.

### type Array[Any]

A Array is a dynamic, 0-indexed array of objects with type Any. It will try to use existing space when appending items, but once space is used up it will perform a resize+copy and return a pointer to the new Array. For this reason you should always capture the return value of functions that modify Arrays such as `append`.

**Fields**

None

**Functions**

None

**Examples**

{% highlight golang %}
{% raw %}
{{$array := Array}}
{{$array = append $array 1 2 "hello" true false 4 5 6}}
{{contains $array 1}} // true
{{contains $array 100}} // false
{{index $array 2}} // returns "hello" which exists at index 2 in the array
{% endraw %}
{% endhighlight %}

### type Map[Comparable]Any

A Map maps any comparable type to a value. This can include int, bool, float, strings, pointers, etc. There are certain cases where a new map might get created during a modify operation, so for this reason you should always capture the output of functions that modify maps (similar to `append` with arrays). For keys that use alphanumeric only (starting with alphabetical character), special . syntax is available (see example below).

**Fields**

None

**Functions**

None

**Examples** 

{% highlight golang %}
{% raw %}
// Map key value key value ....
{{$map := Map 512 true "hello" false "world" 5.0}}

{{contains $map 512}} // true
{{contains $map true}} // false
{{contains $map "hello"}} // true
{{contains $map "earth"}} // false

{{$map = insert $map "earth" 81}}
{{$map = remove $map "hello"}}

// Special . syntax for alphanumeric keys (where first character is alphabetical)
{{$map.earth}} // returns 81
{{$map.hello}} // returns empty (nil) since "hello" was removed
{{$map.world}} // returns 5.0
{% endraw %}
{% endhighlight %}

### type SlashCommandInputData

A Array of these objects are passed into slash commands when they accept user input.

**Fields**

.Name | String | Name of the input
.Type | [OptionType](/docs/#enum-optiontype) | Type of the option
.Value | any | Data passed in as user input (see functions for conversion)

**Functions**

Each of these functions will attempt to convert the input to the desired type. Certain ones, such as **.User**, return nil when it fails. This means the underlying type is something different than requested.

.Bool | returns boolean value of option
.Integer | returns int64 value of option
.Float | returns float64 (double) value of the option
.String | returns string value of the option
.User | returns *User object
.Member | returns *Member object
.Role | returns *Role object
.Channel | returns *Channel object

### type AuditLogTriggerData

**Fields**

Action | String | Type of action the entry represents
Reason | String | Optional reason supplied by moderator/admin
Moderator | Member | Moderator responsible (may be nil)
Target | any | Target of the action
Until | Time | For timeouts, represents time when it expires

**Functions**

AsMember | Member | Converts audit log target to a Member object
AsChannel | Channel | Converts audit log target to a Channel object

### type MessageComponentInteractionData

**Fields**

CustomID | String | Trigger-supplied custom ID for the component
ComponentType | [ComponentType](/docs#enum-componenttype) | 
Values | Array[String] | Only filled when ComponentType is SelectMenuComponent (3). Otherwise is nil.

**Functions**

None

### type CmdChoice

**Fields**

.Name | String | **required** | Name that appears in the slash command selection menu
.Value | Any | **required** | Value assigned to the choice for use by the code

**Examples**

{% highlight golang %}
{% raw %}
{{$choice := CmdChoice
    Name: "green-color"
    Value: 192156505070501888
}}
{% endraw %}
{% endhighlight %}

### type CmdOption

**Fields**

.Type | [OptionType](/docs#enum-optiontype) | **required** | Type of data this option can contain. This determines what inputs the Discord UI will accept. 
.Name | String | **required** | Top-level name of the option
.Description | String | **required** | Short explanation of the option
.ChannelTypes | Array[[ChannelType](/docs#enum-channeltype)] | **optional** | An array of channel types that this option accepts
.Required | Bool | **optional** (default false) | Whether this option is required or not
.Choices | Array[OptionType] | **optional** | Selection menu that the user can choose from
.MinValue | Double | **optional** | Minimal value of number/integer option
.MaxValue | Double | **optional** | Maximum value of number/integer option
.MinLength | Int64 | **optional** | Minimum length of string option
.MaxLength | Int64 | **optional** | Maximum length of string option

**Examples**

{% highlight golang %}
{% raw %}
{{$option := CmdOption
    Type: OptionString
    Name: "message"
    Description: "Optional message to include"
    Required: false
}}
{% endraw %}
{% endhighlight %}

### type Embed

**Fields**

All fields for Embed technically optional, but at least one should have data.

.URL | String | **optional** |
.Title | String | **optional** |
.Description | String | **optional** |
.Timestamp | String | **optional** |
.Color | int | **optional** | Integer value of a hex color
.Footer | [EmbedFooter](/docs#type-embedfooter) | **optional** |
.Image | [EmbedImage](/docs#type-embedimage) | **optional** |
.Thumbnail | [EmbedThumbnail](/docs#type-embedthumbnail) | **optional** |
.Video | [EmbedVideo](/docs#type-embedvideo) | **optional** |
.Provider | [EmbedProvider](/docs#type-embedprovider) | **optional** |
.Author | [EmbedAuthor](/docs#type-embedauthor) | **optional** |
.Fields | Array[[EmbedField](/docs#type-embedfield)] | **optional** |

{% highlight golang %}
{% raw %}
{{$e := Embed
    Title: "Embed Title Here"
    Description: "Content here describing things"
}}
{% endraw %}
{% endhighlight %}

### type EmbedFooter

All fields for EmbedFooter technically optional, but at least one should have data.

.Text | String | **optional** |
.IconURL | String | **optional** |
.ProxyIconURL | String | **optional** |

### type EmbedImage

All fields for EmbedImage technically optional, but at least one should have data.

.URL | String | **optional** |
.ProxyURL | String | **optional** |
.Width | int | **optional** |
.Height | int | **optional** |

### type EmbedThumbnail

All fields for EmbedThumbnail technically optional, but at least one should have data.

.URL | String | **optional** |
.ProxyURL | String | **optional** |
.Width | int | **optional** |
.Height | int | **optional** |

### type EmbedVideo

All fields for EmbedThumbnail technically optional, but at least one should have data.

.URL | String | **optional** |
.Width | int | **optional** |
.Height | int | **optional** |

### type EmbedProvider

All fields for EmbedProvider technically optional, but at least one should have data.

.URL | String | **optional** |
.Name | String | **optional** |

### type EmbedField

.Name | String | **required** |
.Value | String | **required** |
.Inline | bool | **optional** |

### type EmbedAuthor

All fields for EmbedAuthor technically optional, but at least one should have data.

.URL | String | **optional** |
.Name | String | **optional** |
.IconURL | String | **optional** |
.ProxyIconURL | String | **optional** |

### type MessageComplex

Allows you to bundle multiple types together into a single message. All fields for EmbedAuthor technically optional, but at least one should have data.

.Content | String | **optional** | Regular text to accompany message
.Embeds | Array[[Embed](/docs#type-embed)] | **optional** | Array of embeds to include with the message (up to 10)
.Components | Array[[Button](/docs#type-button)] or Array[Array[[Button](/docs#type-button)]] | **optional** | Allows for the adding of buttons to the message
.Files | Array[*discordgo.File] | **optional** | Allows privileged code to send file objects with the message

### type EmojiComponent

.Name | String | Name of the emoji as it appears on Discord/as it is named in a Guild
.ID | ID | ID of the emoji which can be found through the Discord client
.Animated | Bool | True if it is an animated emoji

### type Button

.Label | String | **required** | Button name that appears to the user
.Style | [ButtonStyle](/docs#enum-buttonstyle) | **required** | Style/flavor of button
.Disabled | Bool | **optional** | If true, button appears greyed out and unselectable
.URL | String | **optional** | (Valid for ButtonStyle.LinkButton only) Link redirect when the user clicks the button
.CustomID | String | **required** | (Cannot be used if URL is specified) Identifier that can be used by the programmer to know which button is pressed
.Emoji | [EmojiComponent](/docs#type-emojicomponent) | **optional** | Emoji attached to the button label

### type Thread

.Title | String | **required** | Title of the thread
.AutoArchiveDuration | Int | **optional** | Duration in minutes before Discord auto archives the thread
.Private | Bool | **optional** | (Only valid for basic thread) True if thread permission set to private
.Slowmode | Int | **optional** | Duration in seconds where someone must wait before sending a new message
.MessageID | ID | **optional** | (Only valid for message thread) message ID to attach the thread to

### type ForumThread

.Title | String | **required** | Title of the thread
.AutoArchiveDuration | Int | **optional** | Duration in minutes before Discord auto archives the thread
.Slowmode | Int | **optional** | Duration in seconds where someone must wait before sending a new message
.Content | String, [Embed](/docs#type-embed), or [MessageComplex](/docs#type-messagecomplex) | **required** | Content of the thread
.Tags | Array[String] | **optional** | List of tag names to apply to the thread (max 5 per thread)

### type RoleInfo

This is used when creating a new role.

.Name | String | **required** | The role's name
.Color | Integer | **optional** | The color the role should have (as a decimal, not hex)
.Separate | Bool | **optional** | If true, members with this role show up separately on the side bar
.Permissions | Array[[DiscordPermission](/docs/#bitfield-discordpermission)] | **optional** | The overall permissions of the role
.Mentionable | Bool | **optional** | Whether this role is mentionable

### type PermissionOverwrite

.ID | ID | **optional** | ID of the user or role - defaults @everyone
.Deny | Array[[DiscordPermission](/docs/#bitfield-discordpermission)] | **optional** | Array of permissions denied to specified role or user
.Allow | Array[[DiscordPermission](/docs/#bitfield-discordpermission)] | **optional** | Array of permissions allowed to specified role or user

### type TextChannel

.Name | String | **required** | Name of the new channel
.Topic | String | **optional** | Topic of the new channel
.Slowmode | Integer | **optional** | Slowmode (in seconds) for the new channel
.PermissionOverwrites | Array[[PermissionOverwrite](/docs#type-permissionoverwrite)] | **optional** | List of permission overwrites - if left empty, it will sync with parent category
.ParentID | ID | **optional** | ID of the parent category - if empty puts it in the default channel list at the top of the server  
.NSFW | Bool | **optional** | true if NSFW channel, false if not

### type DatabaseEntry

This is only returned from the dbGet* functions. As a result, it can't be created in user code.

.CreatedAt | *time.Time | Timestamp of when the entry was first created or overwritten
.ExpiresAt | *time.Time | Timestamp of when the entry will expire and go out of scope (may be empty if not created with `dbSetExpire`)
.ID        | ID | 64-bit ID component from `dbSet id key data`
.Key       | String | Key component from `dbSet id key data`
.Value     | Any | Data component from `dbSet id key data`

# Functions

### addOptions ...args

Stage: **command creation**

Takes a space-separated list of [CmdOption](/docs#type-cmdoption). This will append the options (in order) to the list of all options for the command currently being created.

### setDefaultPermissions ...args

Stage: **command creation**

Takes a space-separated list of [DiscordPermission](/docs/#bitfield-discordpermission). This will join the options together to represent a default set of permissions that a member should have in order to use this command.

### toString arg

Converts arg to its string representation. Returns empty string if it fails.

### toInt64 arg

Converts arg to int64. Returns 0 if it fails.

### toFloat64 arg

Converts arg to float64 (double). Returns 0.0 if it fails.

### toID arg

Converts arg to an ID type. An ID represents a Discord snowflake, which is a 64-bit value. The underlying type is int64.

### currentTime

Returns current time stamp, converted to US/Denver location.

### toTime arg

Takes a string or int and attempts to convert it to a Time object.

### printf format ...args

Takes a format string and a series of arguments and converts them to a string. This is equivalent to Sprintf in other languages.

### print ...args

Takes a list of arguments and converts them to a string. This is equivalent to Sprint in other languages.

### println ...args

Takes a list of arguments and converts them to a string, inserting a newline at the end. This is equivalent to Sprintln in other languages.

### joinStr separator ...args

Takes a list of arguments and a separator and returns a string where `separator` is inserted between each element in `...args`. If one of the arguments is an Array of strings, it will iterate over each string in the Array and insert the separator.

### joinArray ...arrays

Takes a list of Array and combines them into a single Array. This works by creating a new Array, then iterating over each element of each array in `arrays` and appending them to the new Array.

### flatten array

Takes an Array of Arrays and flattens it down to a single Array. This works by creating a new Array, iterating over each element of each Array in `array` and appending them to the new Array.

### joinMap ...maps

Takes a list of Maps and joins them into a single Map. This works by creating a new Map, iterating over each key/value pair in each of `maps` and inserting them into the new Map.

### sanitize string elems (replaceWith)

Sanitizes `string` by iterating over it and removing any occurrence of any string in `elems`. By default it will delete these occurrences completely, but it is possible to use the optional `replaceWith` argument to replace any occurrences of `elems` with `replaceWith`.

**Examples**

{% highlight golang %}
{% raw %}
// Deletes any occurrences of , or ! - result is "Hello world"
{{sanitize "Hello, world!" (Array "," "!")}}
// Replaces any occurrences of , or ! with ... - result is "Hello... world..."
{{sanitize "Hello, world!" (Array "," "!") "..."}}
// Result of split using "" as separator is equivalent to (Array "," "!" "?" "." "[" "]")
{{sanitize "Hello, world!" (split ",!?.[]" "")}}
{% endraw %}
{% endhighlight %}

### lower string

Takes a string and converts all characters to lowercase.

### upper string

Takes a string and converts all characters to uppercase.

### split string separator

Takes a string and separator and returns a Array.

### trimWhitespace string

### replaceOne string old new

Replaces the first occurrance of `old` with `new` and returns a new string.

### replaceAll string old new

Replaces all occurrances of `old` with `new` and returns a new string.

### add x y ...rest

Takes a list of arguments and returns either an int64 or float64 sum (depends on type of first argument).

### mult x y ...rest

Takes a list of arguments and returns either an int64 or float64 product (depends on type of first argument).

### sub x y

Returns either int64 or float64 subtraction (depends on type of first argument).

### div x y

Returns either int64 or float64 division (depends on type of first argument).

### sqrt num

Takes a float-convertible number and returns the square root.

### cbrt num

Takes a float-convertible number and returns the cube root.

### round num

Takes a float-convertible number and returns a number rounded up or down (depends on the input).

### floor num

Takes a float-convertible number and returns the floor (round down) of the number.

### ceil num

Takes a float-convertible number and returns the ceil (round up) of the number.

### bitwiseAnd x y

Returns x & y as a uint64

### bitwiseOr x y

Returns x | y as a uint64

### bitwiseXor x y

Returns x ^ y as a uint64

### bitwiseNot x

Returns ~x as a uint64

### randChoice array

Takes an Array input and returns a random element from it.

### rand

Returns a random int64.

### randn limit

Takes an int64-convertible limit and returns a random number from [0, limit) non-inclusive.

### contains container item

Takes a container (array, map, string) and an item and returns true if that item is present in the container.

**Examples** 

{% highlight golang %}
{% raw %}
{{contains "hello, world!" "hello"}} {{/* true */}}
{{contains "hello, world!" "earth"}} {{/* false */}}
{{contains (Array 1 2 3 4) 3}} {{/* true */}}
{{contains (SMap "hello" true "world" true) "world"}} {{/* true */}}
{% endraw %}
{% endhighlight %}

### index container key

Performs an index operation with `key` on `container` and returns the value. It works on arrays, maps and strings.

**Examples** 

{% highlight golang %}
{% raw %}
{{$a := Array 1 2 3 4}}
{{$m := Map 1024 true 8192 false}}
{{$sm := SMap "hello" true "world" false}}
{{$msg := print 
    (index $a 2) "\n" // gets index 2 from $a
    (index $m 1024) "\n" // gets key 1024 from $m
    (index $sm "hello") "\n" // gets key "hello" from $sm
}}
{% endraw %}
{% endhighlight %}

### append container ...items

Takes a container (array or bot's runtime array) and a list of items and appends them to the list. You should always capture the return value of `append` since it will often return a new array object. This happens in 2 main situations: 1) the first array ran out of space and needed to be regenerated, and 2) the input was a bot runtime array (passed to your code by the bot), which are immutable and require a full copy in order to modify.

**Examples** 

{% highlight golang %}
{% raw %}
{{$a := Array}}
{{$a = append 1 2 "hello" true}}
{% endraw %}
{% endhighlight %}

### slice container start (end)

This takes an existing string or array and slices it, returning a view into the original container. `end` is an optional index and is exclusive, whereas `start` is a required index and is inclusive. If `end` is not given then the length of the array/string is used in its place.

**Examples** 

{% highlight golang %}
{% raw %}
{{$a := Array true false 3 4 5}}
{{slice $a 1}} // returns array[false 3 4 5]
{{slice $a 1 2}} // returns array[false]
{% endraw %}
{% endhighlight %}

### insert container key value

Attempts to insert `key`-`value` pair into `container`. This works on arrays and maps. In the case of arrays, the index needs to already be a valid location (meaning it will not allocate new memory as part of the operation). Similar to `append`, you should always capture the return value of `insert` because it may allocate a new map. This only happens in cases where you try to modify a bot runtime map which are immutable, meaning it first needs to perform a full copy before you can modify it.

**Examples** 

{% highlight golang %}
{% raw %}
{{$m := Map}}
{{$m = insert $m 1024 true}} // ok
{{$m = insert $m "hello" "world"}} // ok

{$a := Array 1 2 3}
{{$a = insert $a 0 "hello"}} // ok: tries to insert "hello" into array location 0 which already exists
{{$a = insert $a 81 "world"}} // bad: tries to insert "world" into array location 81 which does not exist yet
{% endraw %}
{% endhighlight %}

### remove container key

Attempts to remove the key-value pair from `container` at location `key`. This works on maps only. If you need a general purpose structure that allows you to insert and delete freely, arrays are not a good choice since they only support insert/append. Similar to `insert` and `append`, you should always capture the return value of `remove` since it may allocate a new map. This only happens in cases where you try to modify a bot runtime map which are immutable, meaning it first needs to perform a full copy before you can modify it.

### reverse container

Reverses the contents of `container`, which should be of type string or Array.

**Examples** 

{% highlight golang %}
{% raw %}
{{$m := SMap "hello" true "world" true}}
{{$m = remove $m "hello"}} // removes "hello" from map
{% endraw %}
{% endhighlight %}

### len container

Returns the length of `container`. This works on arrays, maps and strings.

### prefix string prefix

Returns true if string starts with prefix.

### suffix string suffix

Returns true if string ends with suffix.

### getChannel id

Returns *Channel object for given `id`.

### getThread id

Returns *Channel object (threads are represented as Channels in Discord) for given `id`.

### getChannelOrThread id

Returns *Channel object for given `id` whether it is a thread or channel.

### getMember id

Returns *Member object for given `id`.

### sendDM id message

Sends a DM to user represented by `id` with message. Message can either be a String, Embed, or MessageComplex.

### getMessage channelID messageID

Returns a *Message object for the given channelID, messageID pair.

### getChannelMessagesMatching channelID pattern limit

**This requires privileged guild status**

Retrieves up to `limit` messages (max 100) from `channelID` matching regex `pattern`.

### sendMessageRetID channelID message

Sends a message to the given channelID. Message can either be a String, Embed or MessageComplex. Returns the ID of the new message upon success.

### sendMessage channelID message

Same as sendMessageRetID but does not return the ID of the new message.

### editMessage channelID messageID message

Edits the message represented by channelID, messageID pair to contain the new content within message. message can either be a String, Embed or MessageComplex.

### editWebhookMessage channelID webhookID messageID message

Edits a webhook message that was previously sent by the bot (any that contain things like Button components). If you have a *Message object, you can pull out the channel, webhook and message IDs out of it. message can be either a String, Embed or MessageComplex.

### deleteMessage channelID messageID

Deletes the message represented by the channelID, messageID pair.

### getRole roleID

Returns a *Role object for a given roleID.

### giveRole userID roleID

Gives the role represented by roleID to userID.

### takeRole userID roleID

Takes the role represented by roleID from userID.

### hasRole userID roleID

Returns true if userID has roleID and false if not.

### dbGet id key

Retrieves a single database entry represented by id, key pair. ID should be an int64 type whereas key should be a String type.

### dbGetMultiple id limit

Retrieves up to `limit` entries from the database where the id matches. Key is not taken into account at all.

### dbSet id key data

Sets a single database entry represented by id, key pair to be data. ID should be an int64 type whereas key should be a String type.

Keep in mind that by default, all integer types are converted to floating point by the DB manager and time types are converted to string. For this reason, if you are trying to store something like an ID which is an int64, convert it to a string first.

There are conversion functions `toInt64`, `toFloat64`, `toString`, `toTime` you can use for data you retrieve from the database.

### dbSetExpire id key data expireAfter

This is the same as `dbSet`, except it takes an expireAfter argument measured in seconds. After that entry has gone out of scope (expired), calls to `dbGet` with that ID-Key pair will return nil.

### dbDel id key

Deletes a single database entry represented by id, key pair. ID should be an int64 type whereas key should be a String type.

### dbIncr id key amount

Atomically increments a single database entry represented by id, key pair. amount can be any type that can be converted/cast to an int64.

### respond message

**Note** This only works in slash commands and message component interactions.

Creates a webhook response to either a slash command or message component interaction. Message can be either String, Embed or MessageComplex.

### respondEphemeral message

**Note** This only works in slash commands.

This is conceptually the same as `respond` except it shows a message to the user that only they can see and only they can dismiss.

### deleteResponse

Deletes the response to a slash command trigger or message component interaction. This does not work for ephemeral messages since only the user can dismiss those.

### responseID

For a non-ephemeral response, this will return the ID of the message that Discord created.

### mute userID reason duration

Mutes userID for reason for given duration.

### unmute userID

### ban userID reason daysToDelete

When banning a user, having daysToDelete set to a value > 0 indicates that 1 or more days worth of messages should be purged from chat from the user being banned.

### unban userID

### toDuration string

Attempts to convert string to a duration. It accepts formatting such as "1h45m" or "30s" or "10ms".

### createThread channelID thread

`thread` should be either a [Thread](/docs/#type-thread) or a [ForumThread](/docs/#type-forumthread)

### deleteThread thread

`thread` should be either its ID or its channel object (threads are represented as channels in Discord).

### addThreadMember thread member

`thread` should be either its ID or its channel object (threads are represented as channels in Discord).
`member` should either be their Member/User object or their ID.

This function adds someone to a thread, meaning it will show up in their follow list and they will be able to view it (if private thread).

### removeThreadMember thread member

`thread` should be either its ID or its channel object (threads are represented as channels in Discord).
`member` should either be their Member/User object or their ID.

This function removes someone from a thread, meaning it will disappear from their follow list and will become invisible to them (if private thread).

### lockThread threadID

Marks a thread as locked. `threadID` should not refer to an archived thread.

### unlockThread threadID

Marks a thread as unlocked. `threadID` should not refer to an archived thread.

### exec function data (delay)

Executes a trigger from the Function category. `function` should be the name of the function as a string (case sensitive). `data` can contain information you want to pass to the function which will get placed into its `context.ExecData` entry. `delay` is optional and can be left out. It is measured in seconds, with a max of 60.

If delay is either left out or not specified, exec calls the function immediately and returns whatever the function returns (if anything). Otherwise it schedules it to run and returns empty (nil).

### schedUnique function key delayMinutes

This is a premium feature. Executes a trigger from the Function category after `delayMinutes` have passed. The functions are unique by `key` (type: string, case sensitive). These functions will only execute once, and calling `schedUnique` with the same key twice will cancel the first one. In addition, bot restarts will preserve these scheduled functions.

Delay in minutes must be between 1 and 10080.

### createRole role

Creates a new role. `role` should be a [RoleInfo](/docs/#type-roleinfo) object.

### editRoleName roleID newName

Edits the role specified by `roleID` to be named `newName`.

### editRoleColor roleID color

Edits the role specified by `roleID` to be `color`, which should be a decimal (not hex). Use `hexToInt` for quick conversion to decimal.

### moveRoleAfter roleID afterID

Moves the role specified by `roleID` to come after the role specified by `afterID` in the Discord role hierarchy.

### moveRoleBefore roleID beforeID

Moves the role specified by `roleID` to come before the role specified by `beforeID` in the Discord role hierarchy.

### deleteRole roleID

Deletes role specified by `roleID`.

### createChannel channel

`channel` should be of type [TextChannel](/docs/#type-textchannel). Creates a new channel and returns a Discord Channel object upon success (empty if not).

### deleteChannel channelID

Deletes channel represented by `channelID`.

### setChannelName channelID name

Overwrites the specified channel's name with `name`.

### setChannelTopic channelID topic

Overwrites the specified channel's topic with `topic`.

### getPermissionsIn channelID roleOrUserID

Returns a [PermissionOverwrite](/docs/#type-permissionoverwrite) for `roleOrUserID` in `channelID`. If no permission overwrites exist for that role or user, empty (nil) is returned.

### editChannelPermissions channelID permissions

Performs an edit of the specified channel's permissions. `permissions` should be an Array of [PermissionOverwrite](/docs/#type-permissionoverwrite).

If a permission in the array is already present in the channel's permissions, it is overwritten with the new values.

If a permission in the array is not already present in the channel's permissions, it is added.

### updateChannelPermissions channelID addPermissions removeOverwrites

Performs an update of the channel permissions by first removing the existing overwrites for role/user IDs listed in `removeOverwrites`, then it updates the permissions from `addPermission`. This is done by the following:
* If an overwrite is present on the channel that is also in `addPermissions`, it is overwritten to the new value.
* If an overwrite does not exist for the channel that is in `addPermissions`, it is added.

`addPermissions` should be an Array of [PermissionOverwrite](/docs/#type-permissionoverwrite).
`removeOverwrites` should be an Array of either IDs or int64.

### setChannelPermissions channelID permissions

Overwrites all existing channel permissions by setting them to `permissions`. `permissions` should be an Array of [PermissionOverwrite](/docs/#type-permissionoverwrite).

This has the effect of deleting existing channel permissions not present in the array, overwriting the ones that are, and adding the ones that aren't.

### ignoreBots

This is only needed for guilds that run a private custom instance of the bot. It ends execution early if the triggering event was bot-generated.

### downloadAttachment attachment

**Note:** Only bots listed as privileged can run this.

`attachment1` should be of type *discordgo.MessageAttachment. The bot will attempt to download the attachment and return a *discordgo.File if it succeeded. This can then be used with [MessageComplex](/docs#type-messagecomplex) to send it to a channel.