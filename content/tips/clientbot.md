---
title: "Client vs Bot"
date: 2021-02-18T20:05:28Z
---

I often see a lot of people using the wrong one, so here's a guide for which you should use.

### discord.Client

The Client class is made specifically to connect to the gateway and handle API requests. As such it's not suitable for the majority of bot applications, and using commands.Bot is preferable since it will handle command parsing, permssion checks, etc for you.

### discord.ext.commands.Bot

The Bot class is designed specifically for you to create functional bots with. It has far more features than the Client class alone, such as commands, cogs, permissions checks, cooldowns, custom checks you can create, command error handling, and more. Generally speaking you should be using this not client, there are very very few occasions you need to use client.

### Feature Comparison

| Client                                            | Bot                                                      |
|---------------------------------------------------|----------------------------------------------------------|
| Connects to the gateway                           | Connects to the gateway                                  |
| Handles API calls, obeying ratelimits             | Handles API calls, obeying ratelimits                    |
| Can handle individual events, i.e. message_create | Can handle individual events, i.e. message_create        |
| Can automatically shard                           | Can automatically shard                                  |
| -                                                 | Can handle user defined commands                         |
| -                                                 | Can put events and commands in separate files using cogs |
| -                                                 | Can do built-in permission checks on commands            |
| -                                                 | Can create custom checks for commands                    |
| -                                                 | Can have easy error handlers for commands                |
| -                                                 | Can create cooldowns for commands                        |

---

## A Note About Naming

If you are using `discord.Client` you should name that variable `client`, likewise, if you are using `commands.Bot` you should name that variable `bot`. If you stick to naming things like this it will help you and other people understand your code, and make it easier for people to help if something breaks. ***Good naming matters. Readability counts.*** Please name your variables correctly, naming a Bot `client` is akin to creating a variable called `integer` and setting it to a string.

Correct:
```py
bot = commands.Bot(command_prefix="!")
```

Incorrect:
```py
client = commands.Bot(command_prefix="!")
```