---
title: "12 - Error Handling"
date: 2021-02-17T12:49:32Z
description: "Okay, we've made it to part 12! Be forewarned, this is going a long one. This part is going to be about how you can make error handlers for your commands, like you saw briefly in the previous part about cooldowns."
---

There are several of ways you can go about error handling with discord.py:

- Per-command error handlers
- Per-cog error handlers
- Global error handlers

In this part I will only be covering per-command and global error handlers, but by all means feel free to go down the per-cog route by reading [the docs](https://discordpy.readthedocs.io/) and experimenting on your own. Again, as a general reminder I advise you don't just copy the code here, rather try to understand it yourself and make your own code. You don't learn by copying, you learn by doing.

### Per-command Error Handling

Okay! The first type of handler we'll make is a simple one for a single command. First let's get the basic code for a cog and a basic command:

```py
from discord.ext import commands
from discord import Member


class ExampleCog(commands.Cog):
    """An example cog to show error handling."""

    def __init__(self, bot: commands.Bot):
        self.bot = bot

    @commands.command(name="example")
    @commands.cooldown(rate=1, per=5, type=commands.BucketType.member)
    @commands.has_guild_permissions(manage_messages=True)
    async def example(self, ctx: commands.Context, member: Member):
        """An example command to show error handling."""

        await ctx.send(f"{1/0}")


def setup(bot: commands.Bot):
    bot.add_cog(ExampleCog(bot))
```

You may have noticed the command doesn't do very much - in fact all it does is raise a `DivisionByZero` error as soon as it runs, but for our purpose this is useful. There are a few different types of error that can be raised here:

- `CommandOnCooldown` due to the `cooldown` decorator
- `MissingPermissions` due to the `has_guild_permissions` decorator
- `MissingRequiredArgument` due to `member` being required
- `ConversionError` due to the `Member` typehint on `member`
- `CommandInvokeError` due to the content of the command raising `DivisionByZero`

{{< tip "info" >}}
To get the original error from a `CommandInvokeError`, you can use its `original` attribute, which returns the exception that cause it.
{{< /tip >}}

Now, on to the error handler itself. The first thing we need to do to add the error handler is to tell discord.py we want to add an error handler to the command, which we can do by using the command's error decorator:

```py
    # Assume that this is all between the command definition and the setup function.

    @example.error  # Replace `example` with whatever the function name of the command is.
```

This tells discord.py to add the function the decorator decorates as the error handler for the command, and it will be called if any error is raised during the command execution, or indeed pre-invokation if the check functions or conversion fails.

Next, we need to actually add a function that will handle the error:

```py
    @example.error
    async def example_error(self, ctx: commands.Context, error: commands.CommandError):
        """Handle errors for the example command."""
```

Great! As you can see the function takes 2 non-self arguments: `ctx` and `error`, which are the command context and the error which was raised. Now, let's add some logic in to the function to show the relevant information to the user so they know what's gone wrong, as usual I'll explain what everything does after, and I urge you don't copy this code but understand it and work on writing your own handler.

```py
    @example.error
    async def example_error(self, ctx: commands.Context, error: commands.CommandError):
        """Handle errors for the example command."""

        if isinstance(error, commands.CommandOnCooldown):
            message = f"This command is on cooldown. Please try again after {round(error.retry_after, 1)} seconds."
        elif isinstance(error, commands.MissingPermissions):
            message = "You are missing the required permissions to run this command!"
        elif isinstance(error, commands.MissingRequiredArgument):
            message = f"Missing a required argument: {error.param}"
        elif isinstance(error, commands.ConversionError):
            message = str(error)
        else:
            message = "Oh no! Something went wrong while running the command!"

        await ctx.send(message, delete_after=5)
        await ctx.message.delete(delay=5)
```

That's a lot of code, but here's what each part of it does:

- First, `isinstance` is a python method that checks whether an object is an instance of a given class. In this case we're checking whether the error is an instance of various discord.py errors to determine which error message we want to send, as sometimes they have different parameters.
- The first error is `CommandOnCooldown`. Here we want to send how long in seconds until the cooldown is over and the user can use the command again, which we can get by the error's `retry_after` attribute.
- Next, for `MissingPermissions` we just want to send that the user is missing the required permissions. It's also possible to send exactly which permission they lack, but for simplicity I'm going to leave that out here, since by default it uses internal names like `manage_guild`, rather than pretty Discord names like `Manage Server`
- For `MissingRequiredArgument` we want to show which argument is missing using the error's `param` attribute.
- Now, if the error is none of those, we want to send a generic message saying that the command failed, and we send that message with the `delete_after` parameter so that the message deletes itself after 5 seconds.
- Finally, we want the message that caused the error to be deleted too, so we can keep chat clean. We don't want this to be deleted immediately, so we set it to be deleted after a `delay` of 5 seconds, which means the command and error response will be deleted at roughly the same time.

That's essentially it for per-command handlers, of course there are other types of error that can be raised in other circumstances, and I'd highly recommend reading the [discord.py docs on exceptions](https://discordpy.readthedocs.io/en/latest/ext/commands/api.html#exceptions) to find out more info about them.

### Global Error Handling

Okay, per-command handling done, on to global handling! Note that some of the content here does assume that you have read the section on per-command handling, so I'd recommend reading that bit if you haven't so you have the required context, even if you don't intend on using a per-command handler.

The first thing we're going to want to do, as with per-command handling, is create a cog, only this time we won't be putting any commands in it, it will be reserved for just errors. This is not necessary, but it does help to segment your code and keep everything easy to find and separate, which is nice and clean.

```py
from discord.ext import commands


class ErrorHandler(commands.Cog):
    """A cog for global error handling."""

    def __init__(self, bot: commands.Bot):
        self.bot = bot


def setup(bot: commands.Bot):
    bot.add_cog(ErrorHandler(bot))
```

Pretty simple, that's essentially just the standard template for a cog as normal. Next we're going to add a listener to receive the command errors:

```py
    # Assume this is all between the class' constructor and the setup method.
    @commands.Cog.listener()
    async def on_command_error(self, ctx: commands.Context, error: commands.CommandError):
        """A global error handler cog."""
```

The parameters are exactly the same as a command error, but this time instead of using a decorator from a command, we're using a listener, `on_command_error`. As with the per-command error handler we now want to do logic to determine what message to send the user:

{{< tip "warning" >}}
This is not a complete error handler. There are exceptions missing, and I do not intend to include them. This is to dissuade people from simply copying the error handler for themselves. I can't stress enough how important it is for learning that people try things for themselves rather than copying code. For all of the errors you might need to handle here, I advise reading the [discord.py docs on exceptions](https://discordpy.readthedocs.io/en/latest/ext/commands/api.html#exceptions). In addition, some of the errors here have multiple derived exceptions, such as `UserInputError`, by all means make more specific error handlers which have specialised messages for each exception!
{{< /tip >}}

```py
    @commands.Cog.listener()
    async def on_command_error(self, ctx: commands.Context, error: commands.CommandError):
        """A global error handler cog."""

        if isinstance(error, commands.CommandNotFound):
            return  # Return because we don't want to show an error for every command not found
        elif isinstance(error, commands.CommandOnCooldown):
            message = f"This command is on cooldown. Please try again after {round(error.retry_after, 1)} seconds."
        elif isinstance(error, commands.MissingPermissions):
            message = "You are missing the required permissions to run this command!"
        elif isinstance(error, commands.UserInputError):
            message = "Something about your input was wrong, please check your input and try again!"
        else:
            message = "Oh no! Something went wrong while running the command!"

        await ctx.send(message, delete_after=5)
        await ctx.message.delete(delay=5)
```

As you can see, this isn't massively different from the per-command handler, but there are a couple of notable differences:

- We're using a listener for the event, rather than assigning a handler to a specific command.
- There's a possibility we now get `CommandNotFound` errors, so we deal with them (by ignoring them because it get's pretty spammy!)
- In general we're handling the errors in a more generic way, we don't know exactly which exceptions could be raised anymore, so we need to account for that.

Again, there are far, far more errors than this, and doing some fancy error embeds might look nice too, but I'll leave that as an exercise for the reader, for this part is now complete! (p.s. for the embed thing, perhaps try making something like you see below!)

![Error Embed](/images/error_embed.png)

Looks like you're now ready to move on to the next part: permissions!

{{< button "/tutorial/13-permissions" "Next: Permissions" >}}
