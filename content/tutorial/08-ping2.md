---
title: "08 - A Better Ping Command"
date: 2021-02-17T12:49:20Z
---

In [04 - A Ping Command](/tutorial/04-pong) we made a simple ping command that shows the bot's websocket latency to the gateway, which in itself can be useful for seeing the latency, but it doesn't show API ping, which is another major factor in the bot's latency.

To add API latency we're going to send a message, and time how long it takes to send, then we'll edit that message to show the ping. This will be quite a short part, as it is just a modification to the existing ping command. To recap, here's the existing ping command:

```py
    @commands.command(name="ping")
    async def ping(self, ctx: commands.Context):
        """Get the bot's current websocket latency"""
        await ctx.send(f"Pong! {round(self.bot.latency * 1000)}ms")
```

Again, I'll show the updated code, and then explain afterwards what was changed and why:

```py
    import time # Put this *at the top of the file*

    @commands.command(name="ping")
    async def ping(self, ctx: commands.Context):
        """Get the bot's current websocket latency"""
        start_time = time.time()
        message = await ctx.send("Testing Ping...")
        end_time = time.time()

        await message.edit(content=f"Pong! {round(self.bot.latency * 1000)}ms\nAPI: {round((end_time - start_time) * 1000)}ms")
```

Firstly, we've imported the time module from the Python standard library. This means we can get the current UNIX time so that we can time the execution of sending the message, which will give us the API latency.

We set start_time to the time, then we send a message saying that we're testing ping, and then record the end time. This is all fairly simple Python, or content we've covered earlier in the turorial, the main difference being that we store the message object returned by the API in the message variable.

Now we see a new function we haven't used before, editing an existing message. Unlike sending a message, we need to specify that we're editing the content of the message. As with the previous ping command we show the websocket latency, but now we also show the latency to the API, calculated in much the same way.

That's it for this part of the tutorial, not a very long part but now you know how to edit messages, which can be quite useful. Now you can move on to the next part!

{{< button "/tutorial/09-snipe" "Next: What Did That Message Say?" >}}
