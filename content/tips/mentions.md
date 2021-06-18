---
title: "Allowed Mentions"
date: 2021-02-18T20:05:28Z
description: "Allowed mentions are a way of telling Discord that you don't want to ping for certain mentions in your message. You can use them with `discord.AllowedMentions` objects."
---

---

In this bonus section I'll cover the Discord feature of allowed mentions.

### First, what are allowed mentions?

Allowed mentions are a way of telling Discord that you don't want to ping for certain mentions in your message. The different types of ping a message can have are `@role` pings, `@everyone` or `@here` pings, `@user` pings, and reply pings, all of which we can turn on and off pings for when mentioning.

### How do I use them?

There're a couple of ways you can use allowed mentions in discord.py:

#### In the bot's constructor

The first way we can set allowed mentions in the bot's constructor, and these will apply on all messages sent by the bot. To do this we need to import discord.py to use them:

```py
import discord
from discord.ext import commands
```

Next, we need to create the bot:

```py
bot = commands.Bot(
    command_prefix="!",
    allowed_mentions=discord.AllowedMentions(
        users=False,         # Whether to ping individual user @mentions
        everyone=False,      # Whether to ping @everyone or @here mentions
        roles=False,         # Whether to ping role @mentions
        replied_user=False,  # Whether to ping on replies to messages
    ),
)
```

The example above will disable all pings in messages the bot sends, but you can toggle these as you like. Try making your bot send mentions with these settings to see allowed mentions in action!

#### Per message

The next way of setting allowed mentions is when sending a message, or replying to one. Either way it uses the same keyword argument, so I'll just show sending a message normally, and you can adapt that to your own code. Again the first thing we need to do if we havent already is import discord.py so we can access the `AllowedMentions` class:

```py
import discord
from discord.ext import commands
```

Now, I'll assume that we're in a cogso I can create a command like this:

```py
    @commands.command(name="dontpingme")
    async def dont_ping_me(self, ctx: commands.Context):
        am = discord.AllowedMentions(
            users=False,
        )
        await ctx.send(f"Hello, {ctx.author.mention}", allowed_mentions=am)
```

This example only disabled the `users` mention, since it's the only one that will happen, which means now you can run that command and the bot will mention you, but won't ping you. Neat, huh?

{{< tip "info" >}}
Note that when using allowed mentions in a message specifically, any mentions you have set in the `AllowedMentions` object will override those mentions' settings that were set in the bot's constructor.
{{< /tip >}}
