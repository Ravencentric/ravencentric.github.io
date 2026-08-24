+++
author = "Ravencentric"
title = "Dict Patterns Don't Match Dict Shapes"
date = "2026-08-24"
description = "Pattern matching on dictionaries ignores extra keys rather than failing."
tags = [
    "Python",
    "match-case",
    "TIL",
]
+++

While working on my [`privatebin`](https://github.com/Ravencentric/privatebin) library,
I ran into an unexpected behavior when
[pattern matching on a `dict`](https://github.com/Ravencentric/privatebin/blob/cf329a72930b50d881dc04adbaac5a33d0350aa5/src/privatebin/_core.py#L187).

Before I get to that though, let's see how pattern matching works on sequences:

```py
def describe(items: list[str]) -> None:
    match items:
        # Match exactly one item: "deploy"
        case ["deploy"]:
            print("Case 1: Deploying with defaults")

        # Match exactly two items: "deploy" and a string,
        # binding the string to `environment`
        case ["deploy", str(environment)]:
            print(f"Case 2: Deploying to {environment}")

        # Match "deploy", two strings, and zero or more additional items
        # captured in `options`
        case ["deploy", str(environment), str(service), *options]:
            print(
                f"Case 3: Deploying {service} to {environment} "
                f"with options {options}"
            )
```

```py
>>> describe(["deploy"])
Case 1: Deploying with defaults
>>> describe(["deploy", "production"])
Case 2: Deploying to production
>>> describe(["deploy", "production", "api", "--force", "--verbose"])
Case 3: Deploying api to production with options ['--force', '--verbose']

```

Behaves exactly as you would expect, no surprises here.

Let's try the same thing with `dict`:

```py
def describe(items: dict[str, str]) -> None:
    match items:
        case {"deploy": str(environment)}:
            print(f"Case 1: Deploying to {environment}")

        case {"deploy": str(environment), "service": str(service)}:
            print(f"Case 2: Deploying {service} to {environment}")

        case {
            "deploy": str(environment),
            "service": str(service),
            **options,
        }:
            print(
                f"Case 3: Deploying {service} to {environment} "
                f"with options {options}"
            )

```

```py
>>> describe({"deploy": "production"})
Case 1: Deploying to production
>>> describe({"deploy": "production", "service": "api"})
Case 1: Deploying to production

```

The second input never made it to the second case!
Turns out mapping patterns, unlike sequence patterns,
don't require an exact shape: extra keys are simply ignored.

This seems incredibly odd to me because it means there's no way to match the exact shape
without a guard, while there are two ways to spell "match this key, whether or not other
keys are present":

```py
case {"deploy": str(environment)}: ...
case {"deploy": str(environment), **rest}: ...

```

Both of these patterns match the same dictionaries.
`**rest` merely gives you access to the keys that would otherwise be ignored,
which you can just as well ignore.

This made me curious so I decided to read
[PEP-0635](https://peps.python.org/pep-0635/#mapping-patterns)
which says:

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

While I understand the reasoning behind this, I still find the behavior pretty
unintuitive. I naturally assumed I could write `{"deploy": str(environment), **_}` when
I wanted to match `"deploy"` and ignore any extra keys. Surely I'm not the only one
who'd make that assumption.

So this leaves us with the following disparity between sequences and mappings:

```py 
match sequence:
    # Match exactly
    case ["deploy", str(environment)]: ...
    # Match the first two and capture any remaining items in `rest`
    case ["deploy", str(environment), *rest]: ...
    # Match the first two and ignore any remaining items
    case ["deploy", str(environment), *_]: ...
```

```py
match mapping:
    # Match any dict with a "deploy" key containing a string
    # ignoring any extra keys
    case {"deploy": str(environment)}: ...
    # Match any dict with a "deploy" key, capturing any extra keys in `rest`
    case {"deploy": str(environment), **rest}: ...
    # SyntaxError: identical to the first case and serves no purpose
    case {"deploy": str(environment), **_}: .....
```

As I alluded to earlier, there is a way to work around this with an `if` guard. You
first match the required keys, then use the guard to make sure there are no other items
in your dict. Not super complex or anything, but certainly not as elegant as it could
have been. Not to mention, you have to be aware of this behavior in the first place.

```py
match items:
    case {"deploy": str(environment)} if len(items) == 1: ...
```

or, something a bit more DRY:

```py
match items:
    case {"deploy": str(environment), **rest} if not rest: ...
```

The latter is probably better because you don't have to update the number you're
checking.
