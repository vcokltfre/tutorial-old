---
title: "02 - An Overview of Discord"
date: 2021-02-17T12:49:22Z
description: "In this part of the tutorial, I’ll try to give a rough explanation of how Discord functions."
---

If you already have a decent understanding of websockets, the API, and Discord terminology, you can likely move on to [03 - Hello, World!](/tutorial/03-hello)

## 1: The Discord Gateway

The Discord gateway is how your bot receives events from Discord, so understanding it can be useful to understand what a bot's capabilities are and what Discord gives you to work with.

### So, what actually **is** this Discord Gateway you keep mentioning?

The gateway is actually a fairly simple websocket connection to Discord - basically a web request that connects a socket, which then stays open so that Discord can send events to you, rather than you fetching events from Discord. Neat, huh?

While you're using discord.py you don't actually have to worry about connecting to the gateway yourself, because the library does that all for you, but I believe nonetheless it's useful to have an understanding of how the underlying architecture works, which often makes the higher level stuff make more sense.

When discord.py receives an event from the gateway it will have an OPCODE and some event data, and a couple of other pieces of data we won't worry about for now. These tell your client what to do with the payload, such as hearbeating to the gateway to show the client is still alive.

If the OPCODE passed is 0, that means it's a dispatch event, and those are the ones we're interested in, because they contain the interesting events like message creations, member joins, and any other typical Discord event like them.

That's pretty much it for the gateway at this level. There is a lot more detail that can be talked about with the gateway, but that's out of the scope of this tutorial, and if you're interested in further reading the official Discord documentation for the gateway can be found [here](https://discord.com/developers/docs/topics/gateway).

## 2: The API

The Discord API is how our bot talks back to Discord. We've got the events from the gateway and we've processed them, but without the API we can't actually do anything with that. If you've ever sent a message on Discord, added a reaction, renamed a channel, or performed almost any other action, you've made API requests to the Discord API to do that.

Well, bots are much the same, only they don't have access to all the same endpoints as the user client (although, that being said they do have access to a couple of exclusive endpoints, like locking the use of an emoji to a specific role!).

Again, as with the gateway, discord.py handles interaction with the API and abstracts away most of the complexity like passing tokens in headers and handling ratelimits.

## 3: Discord Terminology

In Discord there are a few special words for things which we need to understand and be able to differentiate between.

| Term   | Description                                                                                                                                                                                                                                      |
|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| guild  | The internal word that Discord has for what you know as a server.                                                                                                                                                                                |
| user   | A Discord user, not containing information relating to a guild.                                                                                                                                                                                  |
| member | A Discord user, with guild context. You'll only receive members if you have the server members gateway intent enabled, which will be explained later, or sometimes in other events like voice state updates and as the author in message events. |

And that's pretty much it for this part of the tutorial. I hope this has given you at least some useful knowledge about roughly how Discord works, and now you can move on to the next part!

{{< button "/tutorial/03-hello" "Next: Hello, world!" >}}
