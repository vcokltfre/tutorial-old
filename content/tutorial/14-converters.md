---
title: "14 - Converters"
date: 2021-05-10T16:57:20Z
description: "Converters are an awesome feature built in to discord.py which allow you to typehint a command argument to have the value automatically converted to that type. This part will teach you about converters, their use, and how you can make your own!"
---

{{< tip "info" >}}
The majority of the information in here is taken from the [discord.py documentation on converters](https://discordpy.readthedocs.io/en/stable/ext/commands/api.html#converters), and if you want more detail on a specific converter I suggest you take a look at the relevant section of the docs.
{{< /tip >}}

## Using Built-In Converters

Off the bat, discord.py provides us with a bunch of useful converters which we can use just by importing the relevant objects. The basic syntax required to use a converter is this:

```py
@bot.command(name="converter")
async def converter_command(ctx: Context, member: discord.Member) -> None:
#                                         ^^^^^^^^^^^^^^^^^^^^^^
# This is the part of the code where the converter is used, by typehinting
# member to `discord.Member` we tell discord.py that we want it as a
# Member object, which the converter will try to fulfil.
```

This method of typehinting to convert is the same for all types. The majority of discord.py objects are able to be used as converters, and some basic python types like `str`, `int`, `float`, and `bool` are also accepted.

An example of a command where you might want to use a converter is a warn command. Let's take this example:

```py
@bot.command(name="warn")
async def warn(ctx: Context, member: discord.Member, *, reason: str = None) -> None:
    """Warn a user with an optional reason."""

    # Pretend we have some sort of database entry here

    await member.send(f"You have been warned on {ctx.guild} for: {reason or 'No reason given.'}")
```

{{< tip "warning" >}}
For the sake of brevity, checks have been left out of this command, but in an actual warn command, or any other moderation command, there should be command checks that ensure that the person running the command has the relevant permissions to run it.
{{< /tip >}}

{{< tip "warning" >}}
There are multiple errors that this command may raise which you'll want to handle in your error handler. The two notable ones here are:
- `commands.ConversionError` (raised during argument conversion)
- `discord.HTTPError` (raised while DMing a user with closed DMs or who have blocked the bot)
{{< /tip >}}

This command quite simply takes a member and a reason and sends them a message. Note here that we can just immediately access the member's `send` method, since the converter guarantees we either get a `Member` object, or an exception is raised if conversion fails. In this command, `member` will **never** be an object of any type other than `Member`.

Try making a few of your own commands use converters, if you don't already! A few converters of interest to you might be:
- `discord.TextChannel`
- `discord.Colour`
- `discord.Role`

## Creating a Custom Converter

So, we've covered the stuff discord.py provides by default, but what if you want more than what it gives? Well, fortunately for us discord provides a base converter class which we can subclass to make our own: `discord.ext.commands.Converter`

{{< tip "info" >}}
For the sake of code readability and reuse, I suggest when making converters you put them in their own file and allow them to be imported into relevant cogs.
{{< /tip >}}

The first thing we need to do to make a converter is to import the base class:

```py
from discord.ext.commands import Converter, Context, BadArgument
```

{{< tip "info" >}}
I've also imported `Context` and `BadArgument` here so that I can use them later on in the class.
{{< /tip>}}

Next, we need to make a subclass of the converter. In this example I'm going to make a converter that tries to convert the given argument into an integer from its binary representation. Examples of accepted inputs would be:
- `0100100101`
- `0b1010011101`

```py
class BinaryConverter:
    pass
```

In order for the converter to actually convert the given input into the type we want, we need to implement its `convert` method, which takes two arguments, `ctx` and `argument`. These are what we will use to do the conversion.

```py
class BinaryConverter:
    """Convert the given argument in binary form into an integer."""

    async def convert(self, ctx: Context, argument: str) -> int:
        try:
            return int(argument, 2)
        except:
            raise BadArgument("Invalid binary integer provided.")
```

This is a very simple implementation of a converter, which simply tries to convert to an int in base 2 using the builtin `int`, and raises `BadArgument` as detailed by the docs if the conversion failed. As this is an async function provided with a `Context` object, it's possible to create far more complex converters should you desire, but that is an exercise left to the reader!

To use the converter we typehint with it in exactly the same way as we did before:

```py
@bot.command(name="bintest")
async def bintest(ctx: Context, num: BinaryConverter) -> None:
    """Convert the binary representation of an integer into a native integer."""

    await ctx.send(f"The number is: {num}")
```

{{< tip "warning" >}}
Be careful when typehinting with converters like this. While this is typehinted with `BinaryConverter`, suggesting that the `num` variable will be an instance of it, the provided value will be an `int`, which may be confusing, and your IDE's suggestions will show incorrect results for the argument.
{{< /tip >}}

---

Now that we're done with converters you're ready to move onto the next part of the tutorial: Waiting for events!

{{< button "/tutorial/15-waiting" "Next: Waiting for events" >}}
