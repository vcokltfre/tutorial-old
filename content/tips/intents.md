---
title: "Gateway Intents"
date: 2021-02-18T20:05:28Z
description: "Gateway Intents are a way of only giving your bot the data it needs to reduce computational burden. By default all intents are enabled except for Guild Members and Presences, which need to be explicitly enabled in the developer portal. These intents decide what data your bot is given by the gateway."
images: ["/images/intents.png"]
---

---

Intents are an important part of Discord which I briefly cover in [this tutorial part](/tutorial/07-welcome), but which deserve their own section explaining them in more depth, so here it is!

### 1 - What are Gateway Intents?

Gateway intents are a way of getting only the data your application needs from the Discord gateway. When you first identify with the gateway upon starting your bot you send the intents you are going to use as an integer which represents a field of bits that describe which intents you need.

There are a few different gateway intents that you can select:

| Gateway Intent Name in Docs | Bit Shift | Description                                                                                      | Privileged |
|-----------------------------|-----------|--------------------------------------------------------------------------------------------------|------------|
| `GUILDS`                    | 1 << 0    | Allows you to receive guild events like joining/leaving guilds, role events, and channel events. | No         |
| `GUILD_MEMBERS`             | 1 << 1    | Allows you to receive member events like joins and leaves.                                       | Yes        |
| `GUILD_BANS`                | 1 << 2    | Allowd you to receive ban add and remove events.                                                 | No         |
| `GUILD_EMOJIS`              | 1 << 3    | Allows you to receive emoji updates.                                                             | No         |
| `GUILD_INTEGRATIONS`        | 1 << 4    | Allows you to receive integration update events.                                                 | No         |
| `GUILD_WEBHOOKS`            | 1 << 5    | Allows you to receive webhook update events.                                                     | No         |
| `GUILD_INVITES`             | 1 << 6    | Allows you to receive invite create and delete events.                                           | No         |
| `GUILD_VOICE_STATES`        | 1 << 7    | Allows you to receive voice state update events.                                                 | No         |
| `GUILD_PRESENCES`           | 1 << 8    | Allows you to receive members' presence updates.                                                 | Yes        |
| `GUILD_MESSAGES`            | 1 << 9    | Allows you to receive message events like create and delete from guilds.                         | No         |
| `GUILD_MESSAGE_REACTIONS`   | 1 << 10   | Allows you to receive message reaction events like add and remove on guild messages.             | No         |
| `GUILD_MESSAGE_TYPING`      | 1 << 11   | Allows you to receive typing events in guilds.                                                   | No         |
| `DIRECT_MESSAGES`           | 1 << 12   | Allows you to receive message events like create and delete from DMs.                            | No         |
| `DIRECT_MESSAGE_REACTIONS`  | 1 << 13   | Allows you to receive message reaction events like add and remove on DM messages.                | No         |
| `DIRECT_MESSAGE_TYPING`     | 1 << 14   | Allows you to receive typing events in DMs.                                                      | No         |

{{< tip "info" >}}
Bear in mind that these intents often also determine which data you receive upon connecting to the gateway. For example if you have members intents disabled you will not receive guild member lists upon connecting, in addition to not receiving member events.
{{< /tip >}}

### Enabling Privileged Intents

To enable privileged intents for your application, first head over to the [Discord developer portal](https://discord.com/developers) and click on your application. Next, head to the Bot tab on the left, and scroll down to the privileged intents section:

{{< tip "warning" >}}
Beyond 100 servers you bot will need to be verified and whitelisted for these intents to be able to use them.
{{< /tip >}}

![Privileged Intents](/images/privileged_intents.png)

Now you can select either or both of these intents so that you can use them in your code. Bear in mind that if one of these intents is not enabled on this page you cannot use it when connecting to the gateway.

### Using Intents in Your Bot

Let's start out with a simple bot setup:

```py
from discord.ext import commands

bot = commands.Bot(command_prefix="!")
```

This example has most of the intents enabled, all except members and presences. There are a couple of ways we can enable these intents, the first option being using all intents - which you should only use if you actually use both memebrs and presence intents:

```py
from discord.ext import commands
from discord import Intents

bot = commands.Bot(command_prefix="!", intents=Intents.all())
```

{{< tip "warning" >}}
You shouldn't use all intents like this unless you actually use them!
{{< /tip >}}

The next method is to create a default intents object, and explicitly enable an intent on it, in this example I'll enabled the members intent since this is the most common one to need:

```py
from discord.ext import commands
from discord import Intents

intents = Intents.default()
intents.members = True

bot = commands.Bot(command_prefix="!", intents=intents)
```

That's pretty much it for gateway intents, they're not too complicated but can definitely catch you out if you don't know about them, so watch out for where you might need to change them!