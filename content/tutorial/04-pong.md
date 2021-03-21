---
title: "04 - A Ping Command"
date: 2021-02-17T12:49:20Z
description: "So far we've made a pretty simple bot - it only responds to `!hello` with a static response. Not particularly interesting, is it? Let's fix that! In this part we'll be creating a ping command that shows the bot's gateway websocket latency when you call it."
---

As with the previous part we'll want to import and set up the bot like below:

```py
from discord.ext import commands

bot = commands.Bot(command_prefix="!")

@bot.command(name="hello")
async def hello_world(ctx: commands.Context):
    await ctx.send("Hello, world!")

bot.run("your_token_here")
```
{{< tip "info">}}
This is a insecure way of setting the token used for brevity, please read [this bonus part about storing tokens.](/tips/tokens)
{{< /tip >}}

But now we want to add another command between the hello command and where we run the bot. Note that if you put the command after running the bot it will never be called since running the bot creates a blocking loop, preventing execution of code after it, until the bot shuts down.

The command will look like this:

```py
@bot.command(name="ping")
async def ping(ctx: commands.Context):
    await ctx.send(f"Pong! {round(bot.latency * 1000)}ms")
```

This will send a message that says something along the lines of "Pong! 113ms" which is the amount of time between discord.py sending a gateway heartbeat and it receiving an acknowledgement from the gateway. In the response we're using an f-string (Python 3.6 and above) so that we can use inline code within the string, in this case the bot's latency is measured in seconds, but we want it in milliseconds, so we multiply it by 1000 and round it to remove the decimals.

It's that simple, you've added another command, go ahead and run the bot to try it out! If it works it should look like the following:

![Ping Pong](/images/ping_pong.png)

Well done! You're already 4 parts in, fortunately there's a lot more to learn, so you can now move on to the next part.

{{< button "/tutorial/05-cogs" "Next: Cogs" >}}
