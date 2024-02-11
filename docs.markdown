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

### context.Message

**May be nil if a message did not trigger the code**

.ID | The ID of the message
.ChannelID | The ID of the channel in which the message was sent
.GuildID | The ID of the guild in which the message was sent
.Content | The content of the message
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

A slice of inputs to a Slash Command. Type is [*SlashCommandInputData](/docs#type-slashcommandinputdata).

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

### type Slice[Any]

A Slice is a dynamic array of objects with type Any. It will try to use existing space when appending items, but once space is used up it will perform a resize+copy and return a pointer to the new Slice.

**Fields**

None

**Functions**

.Append ...args | returns Slice | Takes a list of objects to append to the slice

**Examples**

{% highlight golang %}
{% raw %}
{{$array := Slice}}
{{$array = $array.Append 1 2 "hello" true false 4 5 6}}
{{contains $array 1}} {{/* true */}}
{{contains $array 100}} {{/* false */}}
{% endraw %}
{% endhighlight %}

### type SMap[String]Any

An SMap (String Map) maps string keys to any type.

**Fields**

None

**Functions**

.Get key | returns value | Takes a string key and returns its value (or nil)
.Insert key value | returns none | Inserts or updates a key, value pair
.Remove key | returns none | Removes entry from map

**Examples** 

{% highlight golang %}
{% raw %}
{{/* SMap key value key value .... */}}
{{$map := SMap "hello" true "world" 123 "!" 5.0}}
{{contains $map "hello"}} {{/* true */}}
{{contains $map "earth"}} {{/* false */}}
{{$map.Insert "earth" true}}
{{$map.Remove "hello"}}
{% endraw %}
{% endhighlight %}

### type Map[Comparable]Any

A Map maps any comparable type to a value. This can include int, bool, float, pointers, etc.

**Fields**

None

**Functions**

.Get key | returns value | Takes a key and returns its value (or nil)
.Insert key value | returns none | Inserts or updates a key, value pair
.Remove key | returns none | Removes entry from map

**Examples** 

{% highlight golang %}
{% raw %}
{{/* Map key value key value .... */}}
{{$map := Map 512 true "hello" false}}
{{contains $map 512}} {{/* true */}}
{{contains $map true}} {{/* false */}}
{% endraw %}
{% endhighlight %}

### type SlashCommandInputData

A Slice of these objects are passed into slash commands when they accept user input.

**Fields**

.Name | String | Name of the input
.Type | [OptionType](/docs/#enum-optiontype) | Type of the option
.Value | any | Data passed in as user input (see functions for conversion)

**Functions**

.Bool | returns boolean value of option
.Integer | returns int64 value of option
.Float | returns float64 (double) value of the option
.String | returns string value of the option
.User | returns *User object
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
Values | Slice[String] | Only filled when ComponentType is SelectMenuComponent (3). Otherwise is nil.

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
.ChannelTypes | Slice[[ChannelType](/docs#enum-channeltype)] | **optional** | A slice of channel types that this option accepts
.Required | Bool | **optional** (default false) | Whether this option is required or not
.Autocomplete | Bool | **optional** (default false) | ...
.Choices | Slice[OptionType] | **optional** | ...
.MinValue | Double | **optional** | ...
.MaxValue | Double | **optional** | ...
.MinLength | Int64 | **optional** | ...
.MaxLength | Int64 | **optional** | ...

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
.Footer | EmbedFooter | **optional** |
.Image | EmbedImage | **optional** |
.Thumbnail | EmbedThumbnail | **optional** |
.Video | EmbedVideo | **optional** |
.Provider | EmbedProvider | **optional** |
.Author | EmbedAuthor | **optional** |
.Fields | Slice[EmbedField] | **optional** |

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

### lower string

Takes a string and converts all characters to lowercase.

### upper string

Takes a string and converts all characters to uppercase.

### split string separator

Takes a string and separator and returns a Slice.

### trimWhitespace string

### replaceOne string old new

Replaces the first occurrance of `old` with `new` and returns a new string.

### replaceAll string old new

Replaces all occurrances of `old` with `new` and returns a new string.

### add ...args

Takes a list of float-convertible arguments and returns a float64 (double) sum.

### mult ...args

Takes a list of float-convertible arguments and returns a float64 (double) product.

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

### randChoice slice

Takes a Slice input and returns a random element from it.

### rand

Returns a random int64.

### randn limit

Takes an int64-convertible limit and returns a random number from [0, limit) non-inclusive.

### contains container item

Takes a container (slice, map, string) and an item and returns true if that item is present in the container.

**Examples** 

{% highlight golang %}
{% raw %}
{{contains "hello, world!" "hello"}} {{/* true */}}
{{contains "hello, world!" "earth"}} {{/* false */}}
{{contains (Slice 1 2 3 4) 3}} {{/* true */}}
{{contains (SMap "hello" true "world" true) "world"}} {{/* true */}}
{% endraw %}
{% endhighlight %}

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

### sleep seconds

Instructs the current code to sleep for `seconds` amount of time. Minimum is 1 and maximum is 60.

### dbGet id key

Retrieves a single database entry represented by id, key pair. ID should be an int64 type whereas key should be a String type.

### dbGet id key data

Sets a single database entry represented by id, key pair to be data. ID should be an int64 type whereas key should be a String type.

### dbDel id key

Deletes a single database entry represented by id, key pair. ID should be an int64 type whereas key should be a String type.

		"respond":          exec.tmplInteractionRespond,
		"respondEphemeral": exec.tmplInteractionRespondEphemeral,
		"deleteResponse":   exec.tmplDeleteResponse,
		"responseID":       exec.tmplResponseID,

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