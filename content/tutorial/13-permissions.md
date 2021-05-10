---
title: "13 - Permissions"
date: 2021-04-24T16:57:20Z
description: "Often when creating bots you'll need to restrict a command to only allow certain people to use it. In this part I'll detail several ways you can use permissions checks in discord.py to do this."
---

There are a few different ways to use command permissions in discord.py, but here are the decorators we'll be using today:

- `has_role()`
- `has_any_role()`
- `has_permissions()`
- `has_guild_permissions()`
- `is_owner()`

I'll also show how to create a custom check decorator yourself so that we'll be able to use `has_any_permissions()` as a decorator on our commands.

For the duration of the tutorial, the command I'll be using is this command which simply prints 'h' to the console when executed:

```py
@commands.command(name="test")
async def test(self, ctx: commands.Context):
    print("h")
```

## has_role()

First up, the `has_role()` decorator. This checks if you have one specific role, which can be specified by name or ID:

```py
@commands.command(name="test")
@commands.has_role("Moderator")
async def test(self, ctx: commands.Context):
    print("h")
```

This will mean that you can only execute the command if you have the role moderator. You can also use it like below:

```py
@commands.command(name="test")
@commands.has_role(774373485015072801)
async def test(self, ctx: commands.Context):
    print("h")
```

By passing a role ID rather than a role name.

{{< tip "warning" >}}
Where possible you should use role IDs rather than names, because an ID specifices one specific role, whereas a name could change or end up referring to a different role entirely. Use names at your own risk and with caution!
{{< /tip >}}

## has_any_role()

Next, the `has_any_role()` decorator. This is similar to the `has_role()` decorator, except that it checks whether the user has any of the specified roles. Again it is possible but not recommended to use both names and IDs, but I will only show IDs here for brevity:

```py
@commands.command(name="test")
@commands.has_role(774373485015072801, 720725754605994087)
async def test(self, ctx: commands.Context):
    print("h")
```

This now means that anyone with either of the roles `774373485015072801` and `720725754605994087` is able to execute the command.

## has_permissions()

The `has_permissions()` decorator checks if the user running the command has the specified permissions **in the channel where the command is executed**. It takes permissions as keyword arguments like below:

```py
@commands.command(name="test")
@commands.has_permissions(manage_messages=True, manage_webhooks=True)
async def test(self, ctx: commands.Context):
    print("h")
```

This will check if the user has the permissions `manage_messages` and `manage_webhooks` globally (i.e. by role) or by the channel overwrites of the current channel.

## has_guild_permissions()

Next we have `has_guild_permissions()`. This decorator checks if the user has the specified permissions globally - so granted by one of their roles and NOT by channel overwrites. It can be used just like the `has_permissions()` decorator:

```py
@commands.command(name="test")
@commands.has_guild_permissions(manage_messages=True, manage_webhooks=True)
async def test(self, ctx: commands.Context):
    print("h")
```

This will check if the user has the permissions `manage_messages` and `manage_webhooks` globally.

## is_owner()

Finally, `is_owner()`. This decorator checks whether the person executing the command is the owner of the bot (or one of the owners if the bot is on a Discord developer team). It takes no arguments and is simply used like this:

```py
@commands.command(name="test")
@commands.is_owner()
async def test(self, ctx: commands.Context):
    print("h")
```

---

Now that we're done with permissions decorators you're ready to move onto the next part of the tutorial: Converters!

{{< button "/tutorial/14-converters" "Next: Converters" >}}
