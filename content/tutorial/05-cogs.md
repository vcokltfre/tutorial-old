---
title: "05 - Cogs"
date: 2021-02-17T12:49:20Z
description: "Cogs are a very important part of discord.py which allow you to organise your commands into groups - not to be confused with actual command groups, which will be explained later in the tutorial."
---

Cogs represent a fairly drastic change in the way you write commands and bots, so it's good that we're getting into them here before you're too used to sticking the commands in the main file of the bot.

{{< tip "warning" >}}
Cogs require a basic understand of OOP/classes in Python. If you're not familiar with this, check out the first video in [Corey Schafer's OOP Tutorial.](https://www.youtube.com/playlist?list=PL-osiE80TeTsqhIuOqKhwlXsIBIdSeYtc)
{{< /tip >}}

To start out with cogs we're going to abandon the code from the previous sections largely in favour of new commands tailored to cogs. First, I'll show how to make a cog still in the main file of the bot, then I'll show you how to move it into a separate file completely.

Firstly, as with the previous samples, we need to import the commands module of discord.py and create a bot:

```py
from discord.ext import commands

bot = commands.Bot(command_prefix="!")
```

Next, we need to create a class that inherites from commands.Cog that we can put our commands in, and a constructor that takes in the bot as its only argument and saves it (at this point I'll also start adding docstrings, those things between """ """, that explain what cogs or commands are for, and are used in the built-in help command):

```py
class SomeCommands(commands.Cog):
    """A couple of simple commands."""

    def __init__(self, bot: commands.Bot):
        self.bot = bot
```

If you didn't know yet, bot: commands.Bot is known as typehinting, and it gives the IDE you're using (or sometimes discord.py itself) a hint as to what type the argument is. You'll see this come in handy later on when we add arguments of certain types to our commands.

Now we want to add back the commands we had before. For the sake of simplicity, we wont add back the hello command, it was a good starting point but in reality commands with static output aren't that interesting, so we probably wont use them much more.

In cogs, commands have their own way of being defined, which is using the `commands.command()` decorator. It serves the same function as `bot.command()`, however it now works in cogs too:

```py
    @commands.command(name="ping")
    async def ping(self, ctx: commands.Context):
        """Get the bot's current websocket latency."""
        await ctx.send(f"Pong! {round(self.bot.latency * 1000)}ms")
```

Note that we're now using `self` as the first argument of the command function, because we're now in a class.

Now we have another step we have to do before we run the bot (and I promise this extra effort will pay off in the long run when you understand the code better and can find things easier!) which is to add the cog to the bot. We do that like this:

```py
bot.add_cog(SomeCommands(bot))
```

That's it, we've added the cog to the bot, now we can run it in the normal way and test out our ping command now using cogs!

```py
bot.run("your_token_here")
```
{{< tip "info">}}
This is a insecure way of setting the token used for brevity, please read [this bonus part about storing tokens.](/tips/tokens)
{{< /tip >}}

Great! It works! But we haven't really solved the problem we had before, which was having everything in one file, in fact we've added to the amount of code in the single file, which is doing the exact opposite of what we were trying to do. So, how do we solve this? Well, with more files of course! discord.py's Bot provides another useful function which is load_extension(), which will load cogs from another file.

To start with, let's set up the main bot file, we'll call it bot.py from now on with the following code (this is not the final code for this file, we'll update it in a bit to reflect the changes we are about to make):

```py
from discord.ext import commands

bot = commands.Bot(command_prefix="!")

bot.run("your_token_here")
```
{{< tip "info">}}
This is a insecure way of setting the token used for brevity, please read [this bonus part about storing tokens.](/tips/tokens)
{{< /tip >}}

Basically, we've stripped most of the logic out because we're going to move that to another file, somecommands.py, so let's create that file and add some code:

```py
from discord.ext import commands # Again, we need this imported


class SomeCommands(commands.Cog):
    """A couple of simple commands."""

    def __init__(self, bot: commands.Bot):
        self.bot = bot

    @commands.command(name="ping")
    async def ping(self, ctx: commands.Context):
        """Get the bot's current websocket latency."""
        await ctx.send(f"Pong! {round(self.bot.latency * 1000)}ms") # It's now self.bot.latency
```

{{< tip "warning" >}}
As you can see in the above examples, cog classes must derive from `commands.Cog` else they will not work.
{{< /tip >}}

Finally for this file, we need to add a setup function so that discord.py can load the cog:

```py
def setup(bot: commands.Bot):
    bot.add_cog(SomeCommands(bot))
```

{{< tip "warning" >}}
The name of this function **must** be `setup` as this is what discord.py will use to load it.
{{< /tip >}}

And that's most of the work done to move it into its own file, we just need to update bot.py to make it load this cog, because currently it has no idea there's a cog here that it needs to load. To do this we'll use the `load_extension()` function I mentioned earlier:

```py
from discord.ext import commands

bot = commands.Bot(command_prefix="!")

bot.load_extension("somecommands") # Note, we don't need the .py file extension

bot.run("your_token_here")
```
{{< tip "info">}}
This is a insecure way of setting the token used for brevity, please read [this bonus part about storing tokens.](/tips/tokens)
{{< /tip >}}

That's it for the basics of cogs, you now know how to create a cog, create commands in that cog, and then load the cog into the bot!

{{< tip "info" >}}
## Extra reading

discord.py actually provides us with a way to easily create new bots and cogs through its module's CLI. As a rough overwiew it provides the commands newbot and newcog, which auto generate bots and cogs respectively.

### newbot

This creates a new bot, by default in the current directory, with a given name. The avilable options are:

- name [required] : The bot's name.
- directory [optional, default=.] : The directory to create the bot in.
- prefix [optional, default=$] : The bot's prefix.
- sharded [optional] : Whether the bot should use `AutoShardedClient`
  - Note that sharding likely won't be of use to you until about 2,000 servers, and is enforced by the gateway at 2,500.
- no-git : Whether the project should be created without a git project.

A command to create a typic new bot might look like this:

`python -m discord newbot TestBot --prefix !`

### newcog

This creates a new cog in the cogs folder of the current directory by default, with a given name. The avilable options are:

- name [required] : The cog's name.
- directory [optional, default=cogs] : The directory to create the cog in.
- class-name [optional, default=[name]] : The cog's class name.
- display-name [optional, default=[name]] : The cog's display name.
- hide-commands [optional] : Whether to hide the commands in the cog from the help command.
- full [optional] : Whether to add special cog methods too.

A command to create a typical new cog might look like this:

`python -m discord newcog mycog`
{{< /tip >}}

{{< button "/tutorial/06-online" "Next: Online!" >}}
