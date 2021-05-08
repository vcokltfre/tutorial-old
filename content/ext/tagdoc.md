---
title: "Zeppelin Tag Reference"
date: 2021-05-08T12:18:43+01:00
---

## Formatting Syntax

All elements within `{}` will be interpreted as tag code, rather than raw text, for example `{args.0}` will format the tag with the first given argument.

## Variables

`args` can be used with dot access with an integer to access the argument at that index:
- `{args.0}`
- `{args.n}`

`user` can be used with dot access with a string to access user details:
- `{user.username}` - e.g. `vcokltfre`
- `{user.id}` - e.g. `297045071457681409`
- `{user.discriminator}` - e.g. `6868`
- `{user.bot}` - e.g. `false`
- `{user.system}` - e.g. `false`
- `{user.publicFlags}` - e.g. `256`

`member` can be used with dot access with a string to access member details:
- `{member.user}` - A `user` as described above.
    - Example usage: `{member.user.username}`
- `{member.joinedAt}` - e.g. `1607803386491`

## Functions

### eq

Checks if all the given arguments are equal.

Syntax: `eq(*args)` \
Example: `eq(user.id, "297045071457681409")`

### gt

Checks if the first argument is greater than the second.

Syntax: `gt(arg1, arg2)` \
Example: `gt(10, 5)`

### gte

Checks if the first argument is greater than or equal to the second.

Syntax: `gte(arg1, arg2)` \
Example: `gte(10, 5)`

### lt

Checks if the first argument is less than the second.

Syntax: `lt(arg1, arg2)` \
Example: `lt(3, 5)`

### lte

Checks if the first argument is less than or equal to the second.

Syntax: `lte(arg1, arg2)` \
Example: `lte(3, 5)`

### and

Checks if all the given arguemts evaluate to true.

Syntax: `and(*args)` \
Example: `and(lt(5, 10), gt(10, 5))`

### or

Checks if any of the given arguments evaluate to true.

Syntax: `or(*args)` \
Example: `or(lt(10, 5), lt(5, 10))`

### not

Returns the opposite of the given input. true becomes false and false becomes true.

Syntax: `not(arg)` \
Example: `not(lt(5, 3))`

### if

Does something if the clause evaluates to true else do something else.

Syntax: `if(clause, do, else)` \
Example: `if(eq(user.id, "297045071457681409"), "Hello!", "Bye.")`

### add

Adds all the given arguments together.

Syntax: `add(*args)` \
Example: `add(1, 2, 3)`

### sub (needs clarification)

Subtracts all the given arguments.

Syntax: `sub(*args)` \
Example: `sub(7, 4)`

### mul

Multiplies all the given arguments together.

Syntax: `mul(*args)` \
Example: `mul(5, 6)`

### div (needs clarification)

Divides all the given arguments.

Syntax: `div(*args)` \
Example: `div(15, 3)`

### slice

Gets a slice of a given string.

Syntax: `slice(string, start, end)` \
Example: `slice("Hello!", 1, 4)`

### lower

Gets the lowercase version of a given string.

Syntax: `lower(string)` \
Example: `lower("Hello")`

### upper

Gets the uppercase version of a given string.

Syntax: `upper(string)` \
Example: `upper("Hello")`

### upperFirst | ucfirst

Gets the string with an uppercase first letter.

Syntax: `upperFirst(string)` \
Example: `upperFirst("dragory")`

### rand

Gets a random number with an optional seed in a given range.

Syntax: `rand(from, to[, seed=null])` \
Example: `rand(0, 10)`

### cases

Selects a single item by index from the given arguments.

Syntax: `cases(mod, *cases)` \
Example: `cases(1, "zero", "one", "two")`

### choice

Gets a random item from the given arguments.

Syntax: `choice(*args)` \
Example: `choice("Yes", "No", "Maybe")`

### isMention

Returns whether the given input is a mention.

Syntax: `isMention(string)` \
Example: `isMention("<@297045071457681409>")`

### tag

Gets the content of another tag, passing the provided subargs.

Syntax: `tag(name, *subargs)` \
Example: `tag("test")`

### set

Sets an ephemeral variable to a given value.

Syntax: `set(name, value)` \
Example: `set("test", 1)`

### get

Gets the value of an ephemeral variable.

Syntax: `get(name)` \
Example: `get("test")`

### setr

Sets an ephemeral variable to a given value and returns the result.

Syntax: `setr(name, value)` \
Example: `setr("test", 1)`

## Examples

Soon:tm:
