---
title: "06 - Online!"
date: 2021-02-17T12:49:20Z
---

You've learned how to make commands, and how to make cogs, so now that you know how to use both, let's make something a little more complicated and useful - a command that sets your bot's status.

For this part we'll start off with the same files we ended with in the last part, which means we'll be adding our new command to somecommands.py, and in fact we won't even need to change the main bot file, that can stay just as it is.

To recap, here's the somecommands.py file from the last part:

```py
from discord.ext import commands # Again, we need this imported


class SomeCommands(commands.Cog):
    """A couple of simple commands."""

    def __init__(self, bot: commands.Bot):
        self.bot = bot

    @commands.command(name="ping")
    async def ping(self, ctx: commands.Context):
        """Get the bot's current websocket latency."""
        await ctx.send(f"Pong! {round(self.bot.latency * 1000)}ms")


# Now, we need to set up this cog somehow, and we do that by making a setup function:
def setup(bot: commands.Bot):
    bot.add_cog(SomeCommands(bot))
```

To add the new status command we want to put it just after the ping command - inside the class. For this command we'll be introduced to a couple of new things. Firstly, we're going to need to import the main discord.py library to use a certain class from it, and second this command needs to take in text to set the status to.

To start with, you'll need to add this line to the top of your file to import the main discord.py library:

```py
import discord
```

Now you can add a new command under ping that lets you change the status, which will look like this (don't worry, I'll explain all the new stuff we see after the code):

```py
    @commands.command(name="setstatus")
    async def setstatus(self, ctx: commands.Context, *, text: str):
        """Set the bot's status."""
        await self.bot.change_presence(activity=discord.Game(name=text))
```

The first new thing here is this bit:
```py
, *, text: str
```

This essentially takes all user input after the command and passes it as the text parameter. This is where it's useful to have the typehints I mentioned earlier, as it tells discord.py's converters which type to convert the arguments given into.

Next, we have a `change_presence` function on the bot. In Discord a status is more broadly known as a presence and can include info such as Spotify statuses for non bot users. `change_presence`, as its name suggests, changes the bot's status to the value given. In this case we give it a `discord.Game` object whose name is whatever text we gave, which will make the status look like "Playing {text}"

{{< tip "info" >}}
Note that in this function we dont use `ctx`, but it still needs to be part of the function defintion, as discord.py always passes it first in commands.
{{< /tip >}}

Now you should be able to test it out and see the bot set its status to something you type in, if you run a command like `!setstatus Minecraft`.

And that's the end of part 6! Now you can move on to the next part - Welcome.

{{< button "/tutorial/07-welcome" "Next: Welcome" >}}
