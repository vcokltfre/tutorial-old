---
title: "07 - Welcome"
date: 2021-02-17T12:49:20Z
description: "Ok, so far we've learned how to make a bot, add commands, use cogs, and change the status. Let's combine some of this and a bit of new stuff to make something that welcomes users with a custom message when they join a server."
---

To start out, we'll once again use the same code from the previous part and add to that. First, we have a modification we need to make to the bot.py file. Up until this point we've been using the default gateway intents that discord.py sends to the gateway - which is all except the privileged intents (server members and presences).

Before we can proceed to modify the code, we need to enable these privileged intents on the Discord developer portal. Go to your bot's bot page on the portal, and scroll down to the privileged intents section:

{{< tip "warning" >}}
Beyond 100 servers you bot will need to be verified and whitelisted for these intents to be able to use them.
{{< /tip >}}

![Privileged Intents](/images/privileged_intents.png)

Now, you need to enable the server members intent, and click save to update the application.

![Privileged Intents](/images/privileged_intents_enabled.png)

{{< tip "warning" >}}
Intents exist for a good reason - to reduce computational burden. On larger bots it may be an issue to have these intents enabled as it means Discord sends a lot more data to the bot, which may overwhelm it, although this should not be an issue for small or even fairly large bots.
{{< /tip >}}

{{< tip "info" >}}
Even without the server members intent enabled, Discord will sometimes still provide you with members, such as in messages, voice state updates, etc. This means that you can often use them as members in things like commands, and they will populate the member cache too. The main thing to note is that with members intent disabled, you will not initially be given the member list, you will not receive member updates such as joins/leaves/nickname changes, and you will not be able to fetch members from the API.
{{< /tip >}}

With that done, let's get back to the code - this will be the last time we have to visit the developer portal for a while. First, we need to tell discord.py to use these intents when connecting to the gateway, which we'll do by creating an intents object with the default intents, and enabling members intent, after which the bot file will look like this:

```py
import discord
from discord.ext import commands

intents = discord.Intents.default()
intents.members = True

bot = commands.Bot(command_prefix="!", intents=intents)

bot.load_extension("somecommands")

bot.run("your_token_here")
```
{{< tip "info">}}
This is a insecure way of setting the token used for brevity, please read [this bonus part about storing tokens.](/tips/tokens)
{{< /tip >}}

That's all that needed to enable intents, and now we can harness the full power of Discord gateway events! Now, we have to create the code that welcomes people, which we will stick in the somecommands.py for now, but you're free to make another file and another cog, and load it in the same way as before.

Again, this will be within the class, just under the commands we already have - but wait, this isn't a command, its an event. Luckily discord.py has us covered for events too, and provides listeners so that we can use them.

As with previous examples I'll show the code we need to use and then explain what the new bits do afterwards:

```py
    @commands.Cog.listener()
    async def on_member_join(self, member: discord.Member):
        channel = self.bot.get_channel(799309066202775624)

        if not channel:
            return

        await channel.send(f"Welcome, {member}!")
```

Ok, quite a lot to cover with this one. First, we have a new decorator, commands.Cog.listener(). This is like commands.command(), only it now receives different gateway events rather than messages, in this case the member join event (a full list of events can be found [here](https://discordpy.readthedocs.io/en/latest/api.html#event-reference) in the documentation) which gives us details about a member joining a guild.

Next, we get a channel from the bot, this is the channel that we'll send the welcome message in. To do this we use `self.bot.get_channel()` to get a channel from the bot's cache of channels.

{{< tip "info" >}}
Note that I explicitly use the word `get` here, not `fetch`. This is because in discord.py, and now throughout this tutorial, `get` will refer to fetching something from the local cache, and `fetch` will mean fetching something from the API. It's important to distinguish which is which, because the `get` methods are synchronous, while `fetch` methods are asynchronous and must be awaited.
{{< /tip >}}

Now, you'll likely have noticed the long number in there, that's the channel's unique [snowflake ID.](https://discord.com/developers/docs/reference#snowflakes) All objects in Discord, be it a role, channel, guild, user, etc. have an ID (excluding things like permission overwrites which just map user and role IDs to a set of permissions) which can be found in the client by right clicking the object and clicking copy ID. To have this option you need to have [developer mode enabled.](https://support.discord.com/hc/en-us/articles/206346498-Where-can-I-find-my-User-Server-Message-ID-) Make sure to replace my channel ID with one of your own, or the message won't be sent!

Next, we need to check if we actually have a channel to send to, since it's not guaranteed, so we check if `channel` is truthy, and `return` if it isnt.

Finally, we send a message to the channel which welcomes the user. In f-string expressions or when converting discord.py objects to strings in general, perhaps using `str()`, if the object has a name attribute that's what it will convert to. For members and users however it's slightly more, and it will convert to the username and discriminator, which is the four numbers after the #, so let's take my user object for example, if we do `str(vcokltfre_user)` we'll get `'vcokltfre#6868'` - which in a welcome message is preferable to pinging people when they join, which can be quite annoying. It's also recommended that you dont DM people welcome messages either.

{{< tip "info" >}}
It's possible to mention the user without pinging them using a Discord feature called `allowed mentions` which discord.py has support for. If you would like to learn more about allowed mentions please read [this bonus part.](/tips/mentions)
{{< /tip >}}

{{< tip "warning" >}}
If you do decide to have the bot DM people on join do be careful - Discord will disable and quarantine a bot's access to DMing people if it DMs members too quickly. This applies in general not just with welcome messages.
{{< /tip >}}

That's it for this part - your bot should now have a fully functioning welcome message when new people join the server. If it doesn't work be sure to check that you have used the correct channel ID for the channel you want to send it in, and that the ID is an integer, not a string.

Now you're ready to move on to the next part!

{{< button "/tutorial/08-ping2" "Next: A Better Ping Command" >}}
