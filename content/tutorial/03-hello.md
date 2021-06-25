---
title: "03 - Hello, world!"
date: 2021-02-17T12:49:23Z
description: "In this part, I’ll show you the basics of how to create a super simple bot and get it connected to Discord."
---

From now on it is assumed that you have the discord.py library installed, along with a version of Python which is 3.6 or above, as versions before this don’t have all the features used in this tutorial.

The first step to creating a bot is to import the discord.ext.commands module of discord.py so that we can create the bot:

```py
from discord.ext import commands  # This is the part of discord.py that helps us build bots
```

Epic! Now that we can use the discord.ext.commands module, we need to actually create a bot instance that will run our commands:

```py
bot = commands.Bot(command_prefix="!")
```

As you can see, the first thing we need to do is tell the bot which command prefix to use, otherwise how can it know when a command is called and that it should respond?

{{< tip "info" >}}
This prefix can actually be one of many things, but for the sake of this tutorial I'll just be using the string `"!"` for the prefix. For now just know that it is possible to create more complex prefixes, such as a different prefix for each server.
{{< /tip >}}

The bot constructor is also where we would specify what are known as gateway intents - essentially telling the gateway which events we want. For now we'll ignore this, however it will be covered in [part 7](/tutorial/07-welcome) when we need to use events not given with the default intents.

Next, we want to add a command to the bot so that it can do something. The first command we'll add is a !hello command, that responds with "Hello, world!"

```py
@bot.command(name="hello")
async def hello_world(ctx: commands.Context):
    await ctx.send("Hello, world!")
```

{{< tip "info">}}
By default, the bot will ignore other bots messages, so sending `!hello` from another bot won't trigger the command.
{{< /tip >}}

That's quite a lot to take in, even if you're quite familiar with Python, so allow me to explain what each piece of it does:

- `@bot.command(name="hello")` is a decorator that converts the function below it into a command that you can run from Discord.
- `async def hello_world(ctx: commands.Context):` defines a hello_world function that takes 1 argument - ctx - which is a Context object that's passed with **every** command. All commands will be passed a [Context](https://discordpy.readthedocs.io/en/latest/ext/commands/api.html#discord.ext.commands.Context) object as their first argument.
- `await ctx.send("Hello, world!")` makes an API call to Discord to send a message to the channel the command was run in with the content "Hello, world!" Note that ctx.send() is an alias of ctx.channel.send(), and functions exactly the same, just in a more concise way.

{{< tip "info">}}
Note that explicitly setting the name in the command decorator is optional, as without it the command will inherit the function name, and often will not be needed, whether to use it is up to you.
{{< /tip >}}

Next, we need to run the bot with its token that you got from the developer portal in [01 - Creating a Bot User](/tutorial/01-setup):

```py
bot.run("your_token_here")
```
{{< tip "info">}}
This is a insecure way of setting the token used for brevity, please read [this bonus part](/tips/tokens) about storing tokens.
{{< /tip >}}


This runs the bot with your token, and abstracts away creating an event loop and running the bot through coroutines.

At this point, your bot should be entirely functional, and if you start it up in your preferred method, you should see it come online and answer commands.

{{< tip "info">}}
Common ways of starting your bot:
- running `python bot.py`
- pressing F5 in most IDEs
{{< /tip >}}

![Hello World](/images/hello_world.png)

{{< button "/tutorial/04-pong" "Next: A Ping Command" >}}
