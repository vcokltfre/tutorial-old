---
title: "09 - What Did That Message Say?"
date: 2021-02-17T12:49:20Z
---

In this part I'll introduce a couple of new things we haven't met yet:
- The message delete event
    - and as such also what the message cache is
- Embeds

The aim of this part is to teach you about how the `on_message_delete` event works, and creating an embed with the content of the deleted message to create a !snipe command.

Firstly, we'll take the code from the previous parts. For this example I'll remove the current commands and events from somecommands.py so that we can focus on the main topic.

We will start out with the following code:

```py
import discord
from discord.ext import commands


class SomeCommands(commands.Cog):
    """A couple of simple commands."""

    def __init__(self, bot: commands.Bot):
        self.bot = bot


def setup(bot: commands.Bot):
    bot.add_cog(SomeCommands(bot))
```

Now that we have the base code there are a few things we need to add. Firstly we need to create an attribute of the cog that stores the last deleted message, which we'll set to None by default in the constructor:

```py
    def __init__(self, bot: commands.Bot):
        self.bot = bot
        self.last_msg = None
```

Following this we can move onto creating a listener for the `on_message_delete` event so that we can record when messages are deleted:

```py
    @commands.Cog.listener()
    async def on_message_delete(self, message: discord.Message):
        self.last_msg = message
```

{{< tip "info">}}
Again, typehinting is not strictly needed here, however in my IDE (VSCode) it means I get more useful code suggestions, so in that regard it's very useful.
{{< /tip >}}

Here's where the message cache comes in; this event will only trigger if the message that got deleted is in the bot's internal message cache, which it will only be if that message was received by the bot during it's current session. By default the message cache stores 1,000 messages, but this can be changed in the bot's constructor with the `max_messages` keyword argument. Messages sent before the bot was started won't fire `on_message_delete` events when deleted, nor will they fire `on_message_edit` events. It is however possiblt to listen to the raw delete and edit events, which will give you the raw payload data sent by Discord, without using the internal cache.

Next, we need to add a command that lets us access this message, and then create an embed and send it, but we also need to handle the case that no message has been deleted since the bot started. As normal I'll show the code we're using and explain the new parts afterwards:

```py
    @commands.command(name="snipe")
    async def snipe(self, ctx: commands.Context):
        """A command to snipe delete messages."""
        if not self.last_msg:  # on_message_delete hasn't been triggered since the bot started
            await ctx.send("There is no message to snipe!")
            return

        author = self.last_msg.author
        content = self.last_msg.content

        embed = discord.Embed(title=f"Message from {author}", description=content)
        await ctx.send(embed=embed)
```

There's quite a lot to cover for this command, so we'll start at the beginning. First we check whether there is a `last_message`, if not we send a message saying so, and then `return` from the function so the rest of it doesnt execute.

Next, we set a variable called `content` to the content of the message that was deleted, and `author` to the author of the deleted message. We could do this inline in the embed's constructor, but I've chosen not to here so that we dont have huge lines, but the functionality remains the same.

Now, we create the embed itself. We want to give it the title of who deleted the message, so we use an f-string to put that in, and following that we set the description to the `content` variable we just made. That's about as simple as it gets for embeds, and in the next part I'll cover embeds in a lot more depth.

Finally, we send `embed` to the channel. To do this we need to explicitly state that we want to send an embed with the `embed` keyword argument. If you test your bot now you should see something like this:

![Snipe](/images/snipe.png)

Awesome! That's it for this part of the tutorial, you can now move on to part 10, All About Embeds!

{{< button "/tutorial/10-embeds" "Next: All About Embeds" >}}
