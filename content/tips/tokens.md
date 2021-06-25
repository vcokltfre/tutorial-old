---
title: "Tokens"
date: 2021-02-18T20:05:28Z
description: "Storing your tokens and application secrets securely is very important. In this bonus tip I'll show the most common ways to do so in Python."
---

In this bonus section I'll explain the 3 most common methods of storing credentials and tokens, while this is focused on discord.py everything after this paragraph applies generally to any secret you want to store. Note that for these examples you should assume that there is a bot defined somewhere else in the code, and the line bot.run(token) is at the bottom. I won't show the bot code in each example to show just the necessary information, and make this applicable outside of Discord bots too.

{{< tip "warning" >}}
**You should also be sure when using git to gitignore all of the files you store sensitive information in from any of these examples, to avoide committing them to version control.**
{{< /tip >}}

## .env files and the python-dotenv module

One of the most standard methods in programming of storing your secret static data is in .env files, and this is used for far more than just Discord bots. To store your token in a .env file is quite simple.

Firstly, you need a file named just `.env` which you can use to store the token. This file should look like the following:

```
TOKEN='your_token_here'
```

That's all there is to the file, but to access it in python we need to do a bit more work. First, we need to install the python-dotenv package using `pip install python-dotenv` (or likely `pip3` if you're on Linux) which will install the module. Now we need to import the module and actually load the file:

```py
from dotenv import load_dotenv

load_dotenv() # You can pass the location of the .env file if it's not in a standard location
```

Now that we've imported the module and loaded the file, we need to access the token from it. To do this we need to import the os module so that we can access the environment variables:

```py
from dotenv import load_dotenv
from os import getenv

load_dotenv()

token = getenv("TOKEN")
```

That's it! The token is now loaded and ready to use.

## Importing from python files

Another technique you can use is to import the token from a python file. This is far simpler than loading from a .env file as it requires no external modules and is just a regular python import.

Assuming you have a folder named `private` and in it a file called `config.py`, you can simply use the following code to import the token from it:

```py
from private.config import token
```

Which assumes `config.py` looks like this:

```py
token = "abcd_my_token"
```

## YAML and JSON config files

The last common technique is using YAML and JSON files to store your token. First, I'll assume you have either a `config.yml` or `config.json` file. You're free to use whichever of these two you like, just look at the correct section for your type.

### YAML

First, the file (`config.yml`) should look like the folowing:

```yml
token: "abcd_my_token"
```

Next, you'll need to have the pyyaml module installed, which can be installed using `pip install pyyaml` (or likely pip3 if you're on Linux) which will install the module. Now we need to import and load the data from that file:

```py
from yaml import safe_load
from pathlib import Path

with Path("config.yml").open() as f:
    config = safe_load(f)

token = config["token"]
```

### JSON

For JSON it's more simple than YAML since no non-standard modules are needed, simply create a file (`config.json`) like the following:

```json
{
    "token":"abcd_my_token"
}
```

Next, we just need to load that token:

```py
from json import load
from pathlib import Path

with Path("config.json").open() as f:
    config = load(f)

token = config["token"]
```

## Which to choose

Generally it's up to you which to choose for your project. Each has it's advantages and disadvantages.

### .env:

| Advantages                                                         | Disadvantages                                             |
|--------------------------------------------------------------------|-----------------------------------------------------------|
| Supported widely, including by things like Docker/docker-compose.  | Limited available data types.                             |
| No changes needed when setting the environment variable otherwise. | Requires an additonal module not in the standard library. |

##### Author's note: I actually can't think of more than this for .env, personally I dislike using .env files, but if you have anything else to put here please [open an issue.](https://github.com/vcokltfre/tutorial/issues)

### config.py

| Advantages                                                                        | Disadvantages                                                                   |
|-----------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Easy to import from without any other modules.                                    | Less parsable if other programs need to access the data which aren't in python. |
| More complex data structures and being able to use python objects and operations. |                                                                                 |

### config.json & config.yml

| Advantages                                                | Disadvantages                                                       |
|-----------------------------------------------------------|---------------------------------------------------------------------|
| Generally more complex data structures than .env.         | Not as complex as python structures can be.                         |
| Easy to group data, such as perhaps database credentials. | Requires an additional module not in the standard library for YAML. |
