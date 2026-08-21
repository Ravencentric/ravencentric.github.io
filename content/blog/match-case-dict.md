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

While working on my [`privatebin`](https://github.com/Ravencentric/privatebin) library,
I encountered a surprising behavior when pattern matching on a `dict`.

Before I get to `dict`, let's check out how pattern matching works on sequences:

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

Behaves exactly as you would expect, no surprises here.

Now the same thing with `dict`:

```py
def describe(items: dict[str, str]) -> None:
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

Now let's run this:

```py
>>> describe({"deploy": "production"})
Deploying to production
>>> describe({"deploy": "production", "service": "api"})
Deploying to production

```

The dictionary never made it to the second case! This is how I found out that mapping
patterns, unlike sequences, don't match the shape exactly.

This seems incredibly odd to me because this means it's impossible to match the exact
shape without guards, while there are two ways to spell "give me this key, ignore
everything else":

```py
case {"deploy": str(environment)}: ...
case {"deploy": str(environment), **kwargs}: ...

```

This naturally made me curious and I decided to read
[PEP-0635](https://peps.python.org/pep-0635/#mapping-patterns)
which states:

> The mapping pattern reflects the common usage of dictionary lookup: it allows
> the user to extract some values from a mapping by means of constant/known
> keys and have the values match given subpatterns.
>
> Extra keys in the subject are ignored even if ``**rest`` is not present.
> This is different from sequence patterns, where extra items will cause a
> match to fail. But mappings are actually different from sequences: they
> have natural structural sub-typing behavior, i.e., passing a dictionary
> with extra keys somewhere will likely just work.
>
> Should it be necessary to impose an upper bound on the mapping and ensure
> that no additional keys are present, then the usual double-star-pattern
> ``**rest`` can be used. The special case ``**_`` with a wildcard, however,
> is not supported as it would not have any effect, but might lead to an
> incorrect understanding of the mapping pattern's semantics.

While I agree with this sentiment, this means common case
gets to save a whole `5` characters
while making the uncommon case both unintuitive and 3x longer (`19` characters)
because it now requires an `if` guard

So this leaves us with the following disparity between sequences and mappings:

```py 
match sequence:
    # Matches exactly
    case ["deploy", str(environment)]: ...
    # Match the first two, capture and bind zero or more items to `rest`
    case ["deploy", str(environment), *rest]: ...
    # Match the first two, capture and discard zero or more items
    case ["deploy", str(environment), *_]: ...
```

```py
match mapping:
    # match any dict that has a "deploy" key with string value
    case {"deploy": str(environment)}: ...
    # match any dict that has a "deploy" key with string value 
    # and capture and bind zero or more items to `rest`
    case {"deploy": str(environment), **rest}: ...
    # SyntaxError, because it's identical to the first case
    # and serves no purpose
    case {"deploy": str(environment), **_}: ...

    # What I actually need to write for an exact match
    case {"deploy": str(environment)} if len(items) == 1: ...
```
