---
title: "discord.py Bot Tutorial"
description: "A tutorial on how to use discord.py to create your own Discord bot in Python, written to fix the flaws of many other popular tutorials."
images: ["/images/thumbnail_new.png"]
---

{{< tip "warning" >}}
The discord.py library has now been discontinued by its maintainer. This tutorial will still work for discord.py versions 1.6.0-1.7.3 but some parts may be broken in version 2.0.0 upwards, which is what most forks of the library are based on. Many of these libraries such as pycord, nextcord, and disnake provide shims that allow you to continue using the `discord` namespace, however, and so it is still possible to use most of this tutorial with those libraries too.

Currently I'm unsure on the future of this tutorial. I love making Discord bots, the Python language, and making content to help other people learn about both of those things, but in the current unstable state of the Discord bot ecosystem in Python I don't feel comfortable creating any more content around it until I can be sure that the library I choose to go with will stick around and my work won't be for nothing.

I would, however, like to say that having looked at several of the available libraries (pycord, nextcord, and disnake) I believe there is one clear winner at the moment, due to its similarity in style to the original discord.py, intelligent and sensible design decisions, and generally the direction it's going in: disnake. This is the library that I personally would like to see win this battle, as its design decisions seem well thought out, specially the implementation of slash commands.
{{< /tip >}}

## A tutorial aiming to provide a detailed explanation of how to create a custom Discord bot using the discord.py library.

This tutorial will walk you through all the aspects of creating your own bot, from creating the bot user itself on the Discord developer portal, to a brief overview of the Discord websocket gateway and HTTP API, to programming the bot itself.

---

## Some Notes Before Starting

{{< tip >}}
For this tutorial you will need:
- Python 3.6 or later installed
- discord.py 1.6.0 or later installed
- Intermediate Python experience or a strong will to learn

An incredibly useful resource to you throughout the tutorial will be the [discord.py documentation](https://discordpy.readthedocs.io)

If you find something amiss with the tutorial, or have a suggestion to improve it please create an issue on [the repo](https://github.com/vcokltfre/tutorial/issues).
{{< /tip >}}

{{< tip "warning" >}}
The discord.py library is an advanced Python library. As such a certain amount of intermediate Python knowledge is assumed in this tutorial, and basic Python functionality will be not covered or explained. That being said, if you're willing to learn and search for things you don't understand as you follow along, there's no reason you shouldn't be able to participate.
{{< /tip >}}

{{< tip "warning" >}}
The premise of this tutorial is that you write code yourself, and understand what you are writing. I aim to explain what I'm doing, and clarify the decisions I make, but this tutorial **will not help you** if you choose to copy and paste its content rather than try to understand and work it out for yourself. I highly recommend you play around with the code you write and make it different from what you see here. This is to help you learn how to use discord.py, not to give you the code for a bot.
{{< /tip >}}

---

## Index

- [00 - Credits and Special Thanks](/tutorial/00-credits)
- [01 - Creating a Bot User](/tutorial/01-setup)
- [02 - An Overview of Discord](/tutorial/02-overview)
- [03 - Hello, world!](/tutorial/03-hello)
- [04 - A Ping Command](/tutorial/04-pong)
- [05 - Cogs](/tutorial/05-cogs)
- [06 - Online!](/tutorial/06-online)
- [07 - Welcome](/tutorial/07-welcome)
- [08 - A Better Ping Command](/tutorial/08-ping2)
- [09 - What Did That Message Say?](/tutorial/09-snipe)
- [10 - All About Embeds](/tutorial/10-embeds)
- [11 - Cooldowns](/tutorial/11-cooldowns)
- [12 - Error Handling](/tutorial/12-errors)
- [13 - Permissions](/tutorial/13-permissions)
- [14 - Converters](/tutorial/14-converters)
- [15 - Waiting](/tutorial/15-waiting)

## Extras

- [Tips - Allowed Mentions](/tips/mentions)
- [Tips - Blocking Calls](/tips/blocking)
- [Tips - Client vs Bot](/tips/clientbot)
- [Tips - Cogs vs Main](/tips/cogs)
- [Tips - Gateway Intents](/tips/intents)
- [Tips - Storing Data](/tips/storage)
- [Tips - Tokens](/tips/tokens)

---

{{< button "tutorial/01-setup" "Start the Tutorial!">}}

---

If this tutorial has helped you, please consider supporting me on [Ko-fi](https://ko-fi.com/vcokltfre) so that I can keep working on projects like this :P
