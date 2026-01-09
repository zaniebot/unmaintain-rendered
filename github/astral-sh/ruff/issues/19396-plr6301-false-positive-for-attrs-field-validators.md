---
number: 19396
title: "`PLR6301` false positive for `attrs` field validators passed as arguments"
type: issue
state: open
author: my1e5
labels:
  - rule
  - preview
assignees: []
created_at: 2025-07-17T10:56:37Z
updated_at: 2025-07-17T14:28:19Z
url: https://github.com/astral-sh/ruff/issues/19396
synced_at: 2026-01-07T13:12:16-06:00
---

# `PLR6301` false positive for `attrs` field validators passed as arguments

---

_Issue opened by @my1e5 on 2025-07-17 10:56_

### Summary

This is a follow-up to a previous issue I made about `attrs` validation decorators and PLR6301:

- https://github.com/astral-sh/ruff/issues/12568
## Background
The `attrs` library allows you to define validators for class attributes in two ways:

1. Decorator-based (already solved in previous issue):

```py
from attrs import define, field

@define
class Foo:
    x: int = field()

    @x.validator
    def validate_x(self, attribute, value):
        if value <= 0:
            raise ValueError("x must be a positive integer")
```

2. Field argument-based (the focus of this issue):

```py
from attrs import define, field

@define
class Foo:
    def validate_x(self, attribute, value):
        if value <= 0:
            raise ValueError("x must be a positive integer")

    x: int = field(validator=validate_x)
```
## Problem
Currently, PLR6301 (no-self-use) triggers on methods like `validate_x` in the second example above, suggesting:

> Method `validate_x` could be a function, class method, or static method

However, in this context, `validate_x` must be an instance method (it needs self). See https://www.attrs.org/en/stable/api.html#attrs.field:

> validator (Callable | list[Callable]) –
> 
> Callable that is called by attrs-generated __init__ methods after the instance has been initialized. They receive the initialized instance, the Attribute(), and the passed value.

## Expected behavior
PLR6301 should not trigger for methods that are referenced as validators in `field(validator=...)` calls, just as it should not trigger for methods decorated with `@<field>.validator`.

## MRE

Playground example - https://play.ruff.rs/9fc9950a-e4a9-4a0d-836e-8b55b8e86ba6

## Additional context

This came to my attention because currently `ty` cannot type check decorator-style validators. See

- https://github.com/astral-sh/ty/issues/267

And it was suggested that using `field(validator=...)` is a valid way to workaround this. (https://github.com/astral-sh/ty/issues/267#issuecomment-2959814598)

> If you change your code to the following, then no special magic is required to type check it, and it will work with all conformant type checkers.
>
>    
>     def check_age(self, _: Any, value: int) -> None:
>         ...
>     age: int = field(default=0, validator=check_age)
>    
>
> Even with mypy, this approach is probably preferable because the signature of `check_age` is type checked.



### Version

ruff 0.12.3

---

_Comment by @ntBre on 2025-07-17 14:28_

Thanks for the reports here and in ty! This looks like a much more invasive change than for #12568 because we'll need to inspect the whole class definition, not just one function at a time. But it makes sense to me to handle this case, especially if it's needed to help type checkers.

---

_Label `rule` added by @ntBre on 2025-07-17 14:28_

---

_Label `preview` added by @ntBre on 2025-07-17 14:28_

---
