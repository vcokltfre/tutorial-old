---
title: "Transitioning to Cogs"
date: 2021-02-18T20:05:28Z
description: "Cogs respresent a drastic change to how you write bots in discord.py. As such it's important to know how to use them, and the differences between using cogs and not using them that may catch you out."
#images: ["/images/clientbot.png"]
---

---

Cogs are a very important part of discord.py, which are discussed in [this tutorial part](/tutorial/05-cogs), but that's more of a guide starting from the start, rather than showing how to transfer an existing bot to use cogs, so that's what this bonus part is for!

{{< tip "info" >}}
I'll be using typehinting (e.g `def foo(bar: Baz):`) throughout this part. Not doing so will not cause any difference to the bot, however it will make it so that there are far less useful code suggestions from your IDE, so generally I'd recommend using it where possible.
{{< /tip >}}

Firstly, here's a simple bot which I'll be transitioning to cogs:

```py
from discord.ext import commands
from discord import Message
from dotenv import load_dotenv
from os import getenv

load_dotenv()  # To learn more about this visit https://vcokltfre.dev/tips/tokens/

bot = commands.Bot(command_prefix="!")

@bot.event
async def on_message(message: Message):
    if message.author.bot:
        return

    if "hello" in message.content.lower():  # A simple example, don't want to get too complex
        await message.channel.send("Hi!")

    await bot.process_commands(message)

@bot.command(name="ping")
async def ping(ctx: commands.Context):
    """Show the bot's gateway latency."""
    await ctx.send(f"Pong! {round(bot.latency * 1000)}ms")

bot.run(getenv("TOKEN"))
```

{{< tip "warning" >}}
In case you didn't know, it's very important here in the `on_message` event that we use `bot.process_commands` afterwards, that way we can still handle commands, else no commands will be handled at all. This not an issue when using `@bot.listen()` rather than `@bot.event`, or when using cogs.
{{< /tip >}}

{{< tip "info" >}}
Beyond this point you will need at least a basic understanding of Object Oriented Programming (OOP)/classes in Python. If you're not familiar with this, check out the first video in [Corey Schafer's OOP Tutorial](https://www.youtube.com/playlist?list=PL-osiE80TeTsqhIuOqKhwlXsIBIdSeYtc).
{{< /tip >}}

Next, we can move on to actually creating the cog itself, which we are going to do **in a different file** called `mycog.py`:

```py
from discord.ext import commands


class MyCog(commands.Cog):  # All cogs must inherit from commands.Cog
    """A simple, basic cog."""

    def __init__(self, bot: commands.Bot):
        self.bot = bot


def setup(bot: commands.Bot):
    bot.add_cog(MyCog(bot))
```

The first important thing to note here is that when creating the class we have a `bot` argment in the cog's constructor. This means that we can have cogs in other files without needing to somehow import and reference the bot from the main file.

Next, there's the `setup` function. This is what discord.py looks out for when trying to load the cog. It **must** be called `setup` else the cog will fail to load and you'll get an error.

Now that we have a simple cog we can add in out existing listener and commmand to it:

```py
from discord.ext import commands
from discord import Message


class MyCog(commands.Cog):  # All cogs must inherit from commands.Cog
    """A simple, basic cog."""

    def __init__(self, bot: commands.Bot):
        self.bot = bot

    @commands.Cog.listener()
    async def on_message(self, message: Message):
        if message.author.bot:
            return

        if "hello" in message.content.lower():
            await message.channel.send("Hi!")

    @commands.command(name="ping")
    async def ping(self, ctx: commands.Context):
        """Show the bot's gateway latency."""
        await ctx.send(f"Pong! {round(self.bot.latency * 1000)}ms")


def setup(bot: commands.Bot):
    bot.add_cog(MyCog(bot))
```

There are several key changes here that need to be noted:

1) We're now passing `self` as the first argument to each function, this is because we're inside a class now.
2) We no longer need to call `process_commands` in the `on_message` listener, since now it's a listener rather than an event.
3) When getting the bot's latency we now use `self.bot.latency`, again because we're now inside a class.

Ok great! We have a cog, now we need to load this cog in the main file. I've stripped the event and command out, since they're not in the cog, so here's what we're left with:

```py
from discord.ext import commands
from discord import Message
from dotenv import load_dotenv
from os import getenv

load_dotenv()

bot = commands.Bot(command_prefix="!")

bot.run(getenv("TOKEN"))
```

Now, we need to add just a single line of code to load that cog we just made:

```py
from discord.ext import commands
from discord import Message
from dotenv import load_dotenv
from os import getenv

load_dotenv()

bot = commands.Bot(command_prefix="!")

bot.load_extension("mycog")  # <--- This single line of code to be precise

bot.run(getenv("TOKEN"))
```

{{< tip "warning" >}}
It's highly recommended that you include some error handling around cog loading, but how that's implemented is up to you. It may also be useful to have a list of cogs and iterate over them and call `load_extension` so that you [don't repeat yourself](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself).
{{< /tip >}}

{{< tip "info" >}}
`load_extension` works in a similar way to `import`. For example let's say we have the following directory structure:
```
/
bot.py
  /cogs
    mycog.py
  /utils
    prefix.py
```
We can load `mycog` from `bot.py` by using `load_extension("cogs.mycog")`

There's a lot more you can do with cogs too, such as reloading cogs, or running code on cog load, unload, and reload. For a more comprehensive list of things cogs can do you should see the [discord.py docs page on cogs](https://discordpy.readthedocs.io/en/latest/ext/commands/cogs.html).
