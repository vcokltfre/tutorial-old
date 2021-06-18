---
title: "Storing Data"
date: 2021-02-18T20:05:28Z
description: "As your bot grows in features you'll probably want to store persistent data. When storing persistent data it's important to do so in the correct way - databases. Remember, JSON is NOT a database, and should NOT be used as one."
#images: ["/images/clientbot.png"]
---

---

As your bot grows in features you'll probably want to store persistent data, and it's important to do this in the correct way, else you can make your life developing harder, and possibly compromise the functionality of your bot.

{{< tip "warning" >}}
If you have one takeaway from this, let it be that JSON is **not** a database, and does not work as one, nor does CSV, or plain text files. JSON works well as a data transfer format, or for config files, but is not made for storing changeable persistent data.

In addition to this, spreadsheets are not databases either. Neither Excel nor Google Sheets are acceptable replacements for actual databases. If I hear that you, the reader, is using a spreadhseet as a database I will feel very bad please do not do this!
{{< /tip >}}

## Databases You Can Use

{{< tip "warning" >}}
Note that the following libraries are async libraries. discord.py is an async library too, so the libraries you use inside it should also be async to prevent blocking calls from stopping the event loop doing important things, such as heartbeating to the Discord gateway.
{{< /tip >}}

### PostgreSQL

Postgres is a popular SQL database due to its large feature set and efficiency, and is generally a good choice when storing data in a bot.

To use postgres in your bot you'll want to use a client library such as [asyncpg](https://pypi.org/project/asyncpg/) which provides an easy interface for interacting with Postgres.

### MySQL / MariaDB

MySQL or it's younger sibling MariaDB are also popular SQL databases which are also extremely commonly used. For most bots there will be no noticable difference between MySQL-based and Postgres, so it's really up to personal preference

To use MySQL or MariaDB in your bot you'll want the [aiomysql](https://pypi.org/project/aiomysql/) client library, which also provides an easy interface for interacting with MySQL.

### aiosqlite

SQLite is a simple, fast, local database. It's a SQL database and can be easily used just about anywhere. It has similar use cases to the databases listed above. To use aiosqlite in your own bot you'll need the [aiosqlite](https://readthedocs.org/projects/aiosqlite/) package installed.

### MongoDB

MongoDB is a document store, not a relational database like MySQL or PostgreSQL, which means its usecases are slightly different. MongoDB is primarily for storing JSON-like objects but in a proper database so you don't need to handle file storage and other general shenanigans when storing data.

To use MongoDB in your bot you'll want to use the [motor](https://pypi.org/project/motor/) client library. Motor also provides an easy interface for interacting with MongoDB, but in a very different way to the previous SQL databases mentioned, since it's fundamentally a different kind of database.

## Resources

| Resource                                                      | Description                                               |
|---------------------------------------------------------------|-----------------------------------------------------------|
| [asyncpg docs](https://magicstack.github.io/asyncpg/current/) | The official asyncpg documentation                        |
| [aiomysql docs](https://aiomysql.readthedocs.io/)             | The official aiomysql documentation                       |
| [aiosqlite docs](https://readthedocs.org/projects/aiosqlite/) | The official aiosqlite documentation.                     |
| [motor docs](https://motor.readthedocs.io/en/stable/)         | The official motor documentation                          |
| [PostgreSQL tutorial](https://www.postgresqltutorial.com/)    | A tutorial to help you learn how to use Postgres          |
| [MySQL tutorial](https://www.mysqltutorial.org/)              | A tutorial to help you learn how to use MySQL and MariaDB |
