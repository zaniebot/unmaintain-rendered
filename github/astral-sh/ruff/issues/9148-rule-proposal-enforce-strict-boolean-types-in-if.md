---
number: 9148
title: "Rule proposal: enforce strict boolean types in if statements"
type: issue
state: open
author: valentincalomme
labels:
  - rule
  - type-inference
assignees: []
created_at: 2023-12-15T11:05:30Z
updated_at: 2023-12-18T08:05:15Z
url: https://github.com/astral-sh/ruff/issues/9148
synced_at: 2026-01-07T13:12:15-06:00
---

# Rule proposal: enforce strict boolean types in if statements

---

_Issue opened by @valentincalomme on 2023-12-15 11:05_

A code guideline I enforce in my projects is only to use boolean values or expressions resulting in a boolean in an if statement. So far, I have not found a linter that enforces this automatically, hence my proposal to add this rule to `ruff`.

In essence, I would like a lint rule that allows the following.

```py
if x < 0:
    ...

if isinstance(x, int):
    ...

if True:
    ...
```
And disallows things like this

```py
if [1,2,3]: # list is not a boolean value
    ...

if "Hello world": # str is not a boolean value
    ...

if None:
    ...
```

Don't know if this is something that `ruff` could implement. If this already exists in another linter, I'd be happy to hear it.

---

_Comment by @andersk on 2023-12-15 23:13_

`if [1, 2, 3]` could be detected with simple patterns, but I assume what you’d really like to detect is `if some_module.some_function_that_returns_list()`. That can’t be done at present because Ruff is not a type checker and doesn’t have access to type information. It’d be more in scope for a type checker like mypy.

Note though that [PEP 8](https://peps.python.org/pep-0008/#programming-recommendations) recommends “For sequences, (strings, lists, tuples), use the fact that empty sequences are false”.

---

_Comment by @zanieb on 2023-12-15 23:38_

Thanks for opening an issue! 

This feels contrary to the design of the Python language. I'm curious why you prefer this?

Separately from whether or not it makes sense, I don't think we can actually do a good job implementing this until we have strong type inference.

---

_Label `rule` added by @zanieb on 2023-12-15 23:38_

---

_Label `type-inference` added by @zanieb on 2023-12-15 23:38_

---

_Comment by @andersk on 2023-12-15 23:49_

The motivation does make sense. Python makes it too tempting to abbreviate `if foo is not None` to `if foo`, which introduces a subtle bug if `foo` is (say) an `int | None` that might be `0`.

---

_Comment by @valentincalomme on 2023-12-16 10:54_

It would indeed require type inference, as in my case, I don't care what the value in the if statement is or where it comes from as long as it evaluates to a boolean. 

I would indeed expect to find these checks in `mypy` or even in something like `flake8-bugbear`, as it's not a mistake but a potentially hard-to-debug bug.

As for the motivation, here is how I'd imagine finding this in the `ruff` documentation. (Maybe a good idea to to provide this as a template for issues dealing with rule proposals?)

# implicit-conditional (RUF-0XX)

## What it does

Checks whether non-boolean expressions are used in conditional statements.

## Why is it bad

Although it is allowed in Python, using non-booleans in conditional statements can have unintended consequences. For instance, a developer might intend to perform a null-check, but it might result in False if an integer equals 0, if a list or dictionary is empty, if a string is empty, etc.

## Example

In the following example, an empty list or a None will be treated the same, as `[]` asserts to `False`. But it isn't clear whether it is what we intend to do.

```py
def average(x: list[int] | None = None) -> float | None:
    if x:
        return None
        
    return sum(x) / len(x)
```

If we'd like an empty list or a None to be treated the same, use instead:

```py
def average(x: list[int] | None = None) -> float | None:
    if x is None or len(x) == 0:
        return None
    
    return sum(x) / len(x)
```

And if we don't, use instead:

```py
def average(x: list[int] | None = None) -> float | None:
    if x is None:
        return None
    
    if len(x) == 0:
        raise ValueError("List cannot be empty.")
    
    return sum(x) / len(x)
```

## When not to use it

If you do not rely on optional types or if you want to keep your code short (i.e. in comprehensions), then, this rule might not be as useful to you.

Similar scenarios may arise with:
- collections (`list`, `dict`, `set`, `tuple`, `frozenset`, etc.), as empty collections are `False`
- numbers (`int`, `float`) as `0` or `0.0` are `False`
- strings as empty strings are `False`
- values marked as `Optional`

---

_Referenced in [python/mypy#16734](../../python/mypy/issues/16734.md) on 2024-01-03 15:04_

---
