+++
title = "discord.py Bot Tutorial"
+++

# discord.py Bot Tutorial

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
{{< /tip >}}

{{< tip "warning" >}}
The discord.py library is an advanced Python library. As such a certain amount of intermediate Python knowledge is assumed in this tutorial, and basic Python functionality will be not covered or explained. That being said, if you're willing to learn and search for things you don't understand as you follow along, there's no reason you shouldn't be able to participate.
{{< /tip >}}

{{< tip "warning" >}}
The premise of this tutorial is that you write code yourself, and understand what you are writing. I aim to explain what I'm doing, and calrify the decisions I make, but this tutorial **will not help you** if you choose to copy and paste its content rather than try to understand and work it out for yourself. I highly recomment you play around with the code you write and make it different from what you see here. This is to help you learn how to use discord.py, not to give you the code for a bot.
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

## Extras

- [Tips - Tokens](/tips/tokens)
- [Tips - Allowed Mentions](/tips/mentions)
- [Tips - Client vs Bot](/tips/clientbot)
- [Tips - Gateway Intents](/tips/intents)
- [Tips - Storing Data](/tips/storage)
- [Tips - Cogs vs Main](/tips/cogs)

---

{{< button "tutorial/01-setup" "Start the Tutorial!">}}
