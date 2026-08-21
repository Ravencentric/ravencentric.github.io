+++
author = "Ravencentric"
title = "TODO"
date = "2026-08-21"
description = "TODO"
tags = [
    "Python",
    "match-case",
]
+++

While working on my [`privatebin`](https://github.com/Ravencentric/privatebin)
library, I encountered a surprising behavior when pattern matching on a `dict`.

Before I get to `dict`, let's checkout how pattern matching works on sequences:

```py
def describe(*items: str) -> None:
    match items:
        # Exactly match a sequence with a single item that must be "deploy"
        case ["deploy"]:
            print("Deploying with defaults")

        # Exactly match a sequence with two items: "deploy" and a string,
        # binding the second item to "environment"
        case ["deploy", str(environment)]:
            print(f"Deploying to {environment}")

        # Match a sequence with at least three items: "deploy", two strings,
        # and zero or more additional items, binding them to variables
        case ["deploy", str(environment), str(service), *options]:
            print(
                f"Deploying {service} to {environment} "
                f"with options {options}"
            )

describe("deploy")
#> Deploying with defaults
describe("deploy", "production")
#> Deploying to production
describe("deploy", "production", "api", "--force", "--verbose")
#> Deploying api to production with options ['--force', '--verbose']
```
Behaves exactly you would expect, no surprises here.

Now the same thing with `dict`:

```py
def describe(**items: str) -> None:
    match items:
        case {"deploy": str(environment)}:
            print(f"Deploying to {environment}")

        case {"deploy": str(environment), "service": str(service)}:
            print(f"Deploying {service} to {environment}")

        case {
            "deploy": str(environment),
            "service": str(service),
            **options,
        }:
            print(
                f"Deploying {service} to {environment} "
                f"with options {options}"
            )
```
Now let's run this
```py
>>> describe({"deploy": "production"})
Deploying to production
>>> describe({"deploy": "production", "service": "api"})
Deploying to production
```
The dictionary never made it to the second case!
This is how I found out, mapping patterns, unlike sequences
dont match the shape exactly.

This seems incredibly odd to me because this means
it's impossible to match the exact shape without guards while
there's two ways to spell the "give me this key, ignore everything else"
```py
case {"deploy": str(environment)}: ...
case {"deploy": str(environment), **kwargs}: ...
```
Both of these do the same thing, albeit the latter binds
any extra items to `kwargs` which you can always ignore, or better yet
just bind it to an underscore so your typechecker wont complain about unused variable either.
```py 
case {"deploy": str(environment), **_}: ...
```
