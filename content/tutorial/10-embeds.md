---
title: "10 - All About Embeds"
date: 2021-02-17T12:49:30Z
description: "Nice! You're already 10 parts in, good job! In this part I'm going to show you all about embeds."
---

I won't show the embeds in tandem with the actual bot, since we're focusing on creating them, but to send them you can use the method presented in [09 - What Did That Message Say?](/tutorial/09-snipe#)

For this part I'll assume that you have imported discord at the top of your file.

## Creating an Embed

Creating an embed is a simple as instantiating an `Embed` object like this:

```py
embed = discord.Embed()
```

But that is not a very useful (although still valid) embed, since it's empty. To make it useful we need to add content to the embed. The first way we can do this is with the title attribute:

```py
embed = discord.Embed(title="Hello, world!")
```

Which will create an embed that looks like this:

![Embed](/images/embed_1.png)

But that's not a very interesting embed, there isn't much to it and it doesn't really display anything useful. One way we can add more to it is by adding a description:

```py
embed = discord.Embed(title="Hello, world!", description=":D")
```

Which will create the following embed:

![Embed](/images/embed_2.png)

It still feels like it's missing something important though... Ah yes! Colour! We can give an embed a colour by specifying the `colour` keyword argument, which is an integer (which I'll represent in hexadecimal for readability), or you can pass it a [discord.py colour](https://discordpy.readthedocs.io/en/latest/api.html#discord.Colour). For this example I'll use sky blue, `0x87CEEB`, as it's a nice colour and quite possibly matches the colour of my logo:

```py
embed = discord.Embed(title="Hello, world!", description=":D", colour=0x87CEEB)
```

Which means we now have an embed that looks like this:

![Embed](/images/embed_3.png)

Hmmm... Better, but it still needs more... Let's give it a username and icon:

```py
embed = discord.Embed(title="Hello, world!", description=":D", colour=0x87CEEB)
embed.set_author(name="vcokltfre", icon_url="https://avatars.githubusercontent.com/u/16879430")
```

{{< tip "info" >}}
You can also pass GIFs as parameters for image fields like avatar and thumbnail on embeds.
{{< /tip >}}

There we go, it's starting to look much nicer and more informative and full now, isn't it?

![Embed](/images/embed_4.png)

But there's still more that we can do! Embeds have a whole lot to offer to make an otherwise bland text chat more rich. Let's add some fields to show information. In this example it will be static information, but you can always replace it with dynamic content (think bot ping perhaps? I'll leave that as an exercise to the reader.)

```py
embed = discord.Embed(title="Hello, world!", description=":D", colour=0x87CEEB)
embed.set_author(name="vcokltfre", icon_url="https://avatars.githubusercontent.com/u/16879430")
embed.add_field(name="Field 1", value="Not an inline field!", inline=False)
embed.add_field(name="Field 2", value="An inline field!", inline=True)
embed.add_field(name="Field 3", value="Look I'm inline with field 2!", inline=True)
```

As you can see, now we're able to make far more detailed embeds like this:

![Embed](/images/embed_5.png)

And embeds still have more to offer, so for the sake of not making this part as long as a novel let's add a few now at the same time (we will need to import the datetime module for this due to the timestamp requiring it):

```py
from datetime import datetime

embed = discord.Embed(title="Hello, world!", description=":D", colour=0x87CEEB, timestamp=datetime.utcnow())
embed.set_author(name="vcokltfre", icon_url="https://avatars.githubusercontent.com/u/16879430")
embed.add_field(name="Field 1", value="Not an inline field!", inline=False)
embed.add_field(name="Field 2", value="An inline field!", inline=True)
embed.add_field(name="Field 3", value="Look I'm inline with field 2!", inline=True)
embed.set_footer(text="Wow! A footer!", icon_url="https://cdn.discordapp.com/emojis/754736642761424986.png")
```

![Embed](/images/embed_6.png)

Remember that you need to send the embed to a channel too. This means you need to call `send(embed=embed)` on a messageable object, for example a `TextChannel` object (i.e. `message.channel.send`) or a `Context` object (`ctx.send`) or the embed will not be sent.

There are a few more things that haven't been covered here, but they're generally less commonly used, and if you want to learn more about them, I recommend you read [the discord.py docs on embeds](https://discordpy.readthedocs.io/en/latest/api.html#embed).

Now that we're done with embeds, you can move on to the next part, Cooldowns!

{{< button "/tutorial/11-cooldowns" "Next: Cooldowns" >}}
