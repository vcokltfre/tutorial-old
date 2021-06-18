---
title: "15 - Waiting"
date: 2021-06-18T21:54:38+01:00
description: "Sometimes you need to wait for certain events to happen in a command, such as to listen for a response. This part explains how you can use `wait_for` to do this in your code."
---

Along with many other useful utilities, discord.py provides us with a function, `Bot.wait_for`, which allows us to wait for specific events to happen. In this part I'll demonstrate the use of `wait_for("message", ...)` and `wait_for("reaction_add")`.

The `wait_for` method is an `async` method and takes 3 parameters: the `event` to wait for, an optional `check`, and an optional `timeout`. The `event` tells the method which gateway events to listen to, the `check` is a function that checks whether the event matches a set of requirements, and the `timeout` is how long to wait for before raising an `asyncio.TimeoutError` if no matching events occurr.

## Waiting for messages

First, let's say we want to wait for a confirmation on a command action. I'll use the example of banning a user.

{{< tip "warning" >}}
This command is to demonstrate how to use `wait_for` amd is not intended to be a proper ban command. For this reason, important checks like permission checks have been omitted for brevity, this code (and all other code in this tutorial) should not be directly copied or used in your own bots.
{{< /tip >}}

```py
@commands.command(name="ban")
async def ban(self, ctx: Context, member: Member):
    """Ban a member."""

    message = await ctx.send(f"Are you sure you want to ban {member}?")
    check = lambda m: m.author == ctx.author and m.channel == ctx.channel

    try:
        confirm = await self.bot.wait_for("message", check=check, timeout=30)
    except asyncio.TimeoutError:
        await message.edit(content="Ban cancelled, timed out.")
        return

    if confirm.content == "yes":
        await member.ban()
        await message.edit(content=f"{member} has been banned.")
        return

    await message.edit(content="Ban cancelled.")
```

Thats a pretty long function compared to what I usually put in here, fortunatelt we only have to focus on a couple of key lines, as the rest will be familiar to you already from previous parts.

The first notable line is where we send the original message. Note that I store the returned message object in a variable so that I can reference it later to edit the existing message, rather than sending a new one. This is not strictly necessary, but I find it keeps the chat cleaner if you don't have a confirmation message and additionally a message displaying the result.

Next, we have a lambda check function. This simply takes in an argument, `m`, which is a `discord.Message` object, and makes sure that the message's channel and author match the context's channel and author. This means only the original command author in the same channel can confirm the ban.

Now for the main juice, we have a `try/except` block where we run the `wait_for`. The reason this is wrapped in a `try/except` is so that we can catch the `asyncio.TimeoutError` that will be raised if no matching messages were sent before the `timeout` we set, in this case 30 seconds, expires. In the `except` block we edit the message to show the user that it timed out, and then return to stop execution of the function.

In the `wait_for` we pass it `"message"` as the type of event we want to wait for, `check=check` to give it our check function, and `timeout=30` so that it times out after 30 seconds.

After the `try/except` block we check if the message content is `yes`, if so we ban the user and edit the message to reflect that, else we just edit the message to say that we've cancelled the operation.

## Waiting for reactions

Waiting for messages is pretty useful but also pretty clunky. Instead, we can wait for reactions to make the process cleaner and easier for the user. I will use the same ban example, but this time adapted to use reactions rather than messages. You'll notice the code is very similat to before, but with a few key differences. For brevity I have left out the function definition, as it remains the same as the previous example.

```py
    # Function definition removed for brevity.
    message = await ctx.send(f"Are you sure you want to ban {member}?")
    await message.add_reaction("✅")
    await message.add_reaction("❌")

    check = lambda r, u: u == ctx.author and str(r.emoji) in "✅❌"  # r=reaction, u=user

    try:
        reaction, user = await self.bot.wait_for("reaction_add", check=check, timeout=30)
    except asyncio.TimeoutError:
        await message.edit(content="Ban cancelled, timed out.")
        return

    if str(reaction.emoji) == "✅":
        await member.ban()
        await message.edit(content=f"{member} has been banned.")
        return

    await message.edit(content="Ban cancelled.")
```

And with those simple modifications we've made it work with reactions instead. The first major difference is that it returns two values, the reaction added and the user who added it. This means we need to modify our check function to take both of these values, rather than one message.

Next, we of course need to change the `wait_for` parameters to use `"reaction_add"` rather than `"message"`. We are also now taking two values from this function, so we change `confirm = ` to `reaction, user = `.

The final change is that we need to check the emoji matches, rather than message content, so we modify the if statement to check against the ✅ emoji.

That's it! If you want to read about this in the official documentation you can check that out [here](https://discordpy.readthedocs.io/en/stable/ext/commands/api.html#discord.ext.commands.Bot.wait_for).

---

{{< tip >}}
That's the end of this part, but there aren't any more parts yet! Stay tuned for more updates to this tutorial to teach you more about discord.py. If you have found an error or mistake, please create an issue at https://github.com/vcokltfre/tutorial and thanks for reading!
{{< /tip >}}
