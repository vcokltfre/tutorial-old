---
title: "Blocking Calls"
date: 2021-02-18T20:05:28Z
description: "discord.py is a fundamentally asynchronous library, and as a result of this the use of blocking calls can cause issues in your bots. This bonus part explains how you can avoid such situations."
---

## What is async?

In Python we have a library called `asyncio` which allows the running of multiple tasks and coroutines simultaneously (or at least, the appearance of being simultaneous, but that's outside of the scope of this tutorial). Where traditionally you can only run one piece of code at a time, `asyncio` makes it so that you can have multiple running at once, and uses context switching to jump between the currently in-progress functions.

There are a ton of great PyCon talks about asyncio, notably [this talk](https://www.youtube.com/watch?v=E-1Y4kSsAFc) by David Beazley which explains amazingly well how a coroutine works at its heart. I highly recommend watching this video for a better understanding of the internals of how async works in Python.

## Why is synchronous code an issue?

Synchronous code is a problem in discord.py bots because it blocks the execution of the event loop while the blocking code executes. But why is that such a big issue? Because the websocket connection to Discord is also handled by the event loop. This means if you block the event loop for long enough then discord.py can't receive events from the gateway, or worse, and possibly fatally, it can't send heartbeats to the gatway, which after long enough will cause the gateway to kill the bot's connection to it.

{{< tip >}}
Technically speaking, all code is blocking, some just blocks for less time than others. I say this so that someone doesn't bring this up in the repo issues, because it's out of scope for this tutorial, but if you're interested please do check out the video I linked above about async.
{{< /tip >}}

## What can I do about this?

Put simply: don't use blocking code. If you have loops of long running code, for example, you can add an `await asyncio.sleep(0)` on each loop. This yields control back to the event loop to allow other coroutines to execute. Explicitly using `await` is basically telling the event loop "hey, you've got control back to schedule other coroutines and tasks now", allowing everything to work in parallel.

You should also seek to use async libraries for things such as http requests and database calls.

## Async libraries

Here's a table of async libraries you can use. If you have one to add, please make an issue or pull request on [the repo](https://github.com/vcokltfre/tutorial).

| Name      | Purpose                                                                                                  |
|-----------|----------------------------------------------------------------------------------------------------------|
| aiohttp   | Make async HTTP requests and WebSocket connections. This is the library that discord.py uses internally. |
| httpx     | Another async HTTP library.                                                                              |
| asyncpg   | An async PostgreSQL database client.                                                                     |
| aiomysql  | An async MySQL/MariaDB database client.                                                                  |
| aiosqlite | An async wrapper around sqlite3.                                                                         |
| aiodns    | Async DNS requests so DNS resolution doesn't slow down the event loop.                                   |
| uvloop    | Speed up the event loop using libuv bindings (except on Windows).                                        |
