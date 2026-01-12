```yaml
number: 566
title: "Instantiating a property as a class (not a decorator) ends with `invalid-assignment`"
type: issue
state: closed
author: dmach
labels: []
assignees: []
created_at: 2025-06-02T11:14:26Z
updated_at: 2025-06-02T16:16:47Z
url: https://github.com/astral-sh/ty/issues/566
synced_at: 2026-01-12T15:54:23Z
```

# Instantiating a property as a class (not a decorator) ends with `invalid-assignment`

---

_@dmach_

### Summary

I maintain a code that is an alternative to pydantic - `BaseModel` class you can extend with `Field`s.
The `Field` class inherits from `property` and the typical usage looks like:

```py
class MyModel(BaseModel):
    my_field: str = Field()
```

`ty` errors out on the code with `error[invalid-assignment]`


I put together a snippet you can use to try it out:

```py
class MyProperty(property):
    pass


class Test:
    one: int = MyProperty(lambda self: 1)

    @MyProperty
    def two(self) -> int:
        return 2


test = Test()
assert test.one == 1
assert test.two == 2
```


the result is:
```
error[invalid-assignment]: Object of type `MyProperty` is not assignable to `int`
 --> property.py:6:5
  |
5 | class Test:
6 |     one: int = MyProperty(lambda self: 1)
  |     ^^^
7 |
8 |     @MyProperty
  |
info: rule `invalid-assignment` is enabled by default
```

The expected result is that the error is not reported.
It might be good to compare the type hint of the property with the type hint of the getter instead.

### Version

_No response_

---

_Comment by @carljm on 2025-06-02 16:14_

Thanks for the report!

Does your `BaseModel` class use `typing.dataclass_transform`?

If so, then probably this will become supported as part of the "support fields" item of https://github.com/astral-sh/ty/issues/111

If not, then it should not be supported as described; the error we give in your second simplified example is correct, and both [mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=e3f8f80bc6f7032c96aa0c887a63302e) and [pyright](https://pyright-play.net/?strict=true&code=MYGwhgzhAECyCeAFATgewA4FNkBd4Ap00td4BKALgChpbp1IIrnRHoAVTCHau6VAHaYK0AJYCc0ALxwkxbHnzgAtgCMAJmGgRMIAGYiAjGWZ8AAghQYF8GnXWY90HAHdU%2BHfrLQAtAD4xCV4%2BOmRMHABXZAFoACZmKhwuSRlObnwTRgVnZIA6QUxpGUMqLNwc7lzXVCK4oA) give the same error we do. Absent dataclass-transform (which instructs the type checker to consider the type annotation as describing the instance attribute's type, and allow assigning a different type on the class), this assignment should be an error. The correct typing would be `one: ClassVar[MyProperty] = MyProperty(lambda self: 1)`, and with that type we would correctly model the property's behavior, and your example [passes with no errors](https://play.ty.dev/d621071c-964d-4143-8225-e03b83d59534).

Closing this as a duplicate of #111 

---

_Closed by @carljm on 2025-06-02 16:14_

---
