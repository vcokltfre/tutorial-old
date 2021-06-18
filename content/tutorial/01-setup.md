---
title: "01 - Creating a Bot User"
date: 2021-02-17T12:49:21Z
description: "This section of the tutorial will show you how to create a new bot and add it to your server."
---

{{< tip "warning" >}}
To create Discord applications and bots you will need a verified email address.
{{< /tip >}}

The first thing you need to do to create a bot is head over to the [Discord Developer Portal](https://discord.com/developers) where you'll be asked to log in to your Discord account. Upon logging in you'll see a screen like this:

![Discord Developer Portal](/images/dev_portal_1.png)

Now that you're logged into the developer portal you need to create a new application by clicking the shiny blue `New Application` button in the top right hand corner. After you click it you should see a box like this pop up:

![New Application](/images/dev_portal_2.png)

Be sure to enter a suitable and good name for your bot - and be sure to follow the Discord Terms of Service, so no slurs or harassment. To be clear this applies to the entire tutorial and you should not use any of the knowledge you gain from it to perform actions that are not allowed by it. You can read the Terms of Service [here,](https://dis.gd/terms) and the Developer Terms [here.](https://discord.com/developers/docs/legal)


{{< tip "warning" >}}
As a rough overview here are some things you should know about the ToS:

- Don't abuse the API
- Don't harass users.
  - So don't randomly DM or spam them.
- Don't spam Discord.
  - Generally actions performed by a bot should be caused by some user action like sending a message or adding a reaction.
- Don't post NSFW content in channels not marked as NSFW.
  - This may seem obvious but a lot of bots allow NSFW content to be posted outside of NSFW channels. This is not allowed.
{{< /tip >}}

For this tutorial the bot will be named `WumpusBot` and will be referred to as that elsewhere in the tutorial.

After creating your application you'll see a screen like this:

![General Information](/images/dev_portal_3.png)

{{< tip "info" >}}
You likely want to set your bot to private - at least while testing - so that other people can't add your bot to their servers.
{{< /tip >}}

There's quite a lot of information on this screen, so for now we'll ignore most of it since it's out of scope for the beginning of this tutorial. Click on the `Bot` tab on the left hand side (marked by a jigsaw piece) to switch to the bot page. On this page you'll see a button that says `Add Bot.` Clicking this will prompt you if you're sure you want to create a new bot (you can't delete bots since they're too cool to destroy), click `Yes, do it!` and now you have your very own bot.

After creating a bot your screen will change to look like this:

![Bot](/images/dev_portal_4.png)

Below the username box you'll see a button that says `Copy` which copies your bot's authentication token to your clipboard. You'll want to keep this token safe and on hand so your bot can connect to Discord later on in the tutorial.

Finally for this part, you need to add your bot to your server. To do this you first require the Manage Server permission in whatever server you plan on adding it to. You’ll want to head over to the OAuth2 tab on the left of the developer portal (marked by a wrench), where you’ll be able to choose the scopes you want for your bot. For now, just select the bot scope, as it’s all that will be needed for this tutorial - at least at the beginning.

Now you’ll want to choose permissions for your bot based on what you want it to do. For WumpusBot, I’ll start by giving it Send Messages, Embed Links, Attach Files, Manage Messages, and Add Reactions, although we may need more permissions later, but that will be handled within Discord itself. 

{{< tip "warning" >}}
It is highly recommended that you never give bots the Administrator permission, even if it feels easier than giving the bot just what’s needed. Please do not give your bots administrator.
{{< /tip >}}

In the end your permissions should look like this:

![Bot Permissions](/images/dev_portal_5.png)

Now you can copy the URL in the box above, and paste it into your browser, then follow the steps to add the bot to your server of choice.

That's it for the first part of the tutorial, you've created your very own bot user on Discord. Now you just have to do the fun bit - adding features - after all, what's a bot without features?

{{< tip "warning" >}}
### A quick note on tokens:

You should make an effort to keep your token safe at all times. This means not sharing it with anyone or accidentally uploading it in code samples. If someone gains access to your bot’s token they then have full control of the bot, and can perform actions with it that you may not want. If you believe that your bot’s token was leaked, be sure to go to it’s developer portal page and click the Regenerate button to regenerate the token so that nobody can use the old one.

For more info in how you should store tokens see [this bonus part.](/tips/tokens)
{{< /tip >}}

{{< button "/tutorial/02-overview" "Next: An Overview of Discord" >}}
