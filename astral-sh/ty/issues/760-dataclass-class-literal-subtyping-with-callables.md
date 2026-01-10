```yaml
number: 760
title: dataclass class literal subtyping with callables not working
type: issue
state: closed
author: MatthewMckee4
labels:
  - bug
  - dataclasses
assignees: []
created_at: 2025-07-03T19:27:34Z
updated_at: 2025-07-09T08:04:58Z
url: https://github.com/astral-sh/ty/issues/760
synced_at: 2026-01-10T02:07:36Z
```

# dataclass class literal subtyping with callables not working

---

_Issue opened by @MatthewMckee4 on 2025-07-03 19:27_

### Summary

This snippet does not work
```py
@dataclass
class DC:
    x: DC | None

v: Callable[[DC], DC] = DC
```

The reason is that `__init__` for `DC` is not a `FunctionLiteral`.

Should we allow other types for these constructor functions too? (other than FunctionLiteral and Callable 

https://github.com/astral-sh/ruff/pull/19130 this may also help with allowing any type and converting it to callable

---

_Label `bug` added by @AlexWaygood on 2025-07-03 19:28_

---

_Label `dataclasses` added by @AlexWaygood on 2025-07-03 19:28_

---

_Comment by @erictraut on 2025-07-03 19:41_

You may find [this section of the typing spec](https://typing.python.org/en/latest/spec/constructors.html#converting-a-constructor-to-callable) to be useful here. It covers the rules for converting the constructor of a class object into a callable type for purposes of assignability testing.

---

_Comment by @MatthewMckee4 on 2025-07-03 20:14_

> 2. If the class defines a `__new__` method

Does this mean we should not allow different types of `__new__`?

And same for `__init__`

---

_Comment by @carljm on 2025-07-04 01:40_

We already implement that section of the spec, it's just that our implementation assumes that `__new__`/ `__init__` will be a `FunctionLiteral` type, and the synthesized `__init__` on a dataclass is not. I think ideally we should support any callable type as `__init__` or `__new__`, using a generalized `Type::into_callable_type` method. (But practically speaking, just adding support for `Type::Callable` so we support dataclass synthesized methods would go a long way.)

---

_Comment by @MatthewMckee4 on 2025-07-04 06:56_

Yeah, I'm trying to generalise it to use the into_callable method we just landed

---

_Comment by @MatthewMckee4 on 2025-07-08 13:02_

Just going for supporting 'Callable' as using into_callable has proved to be a lot more work

---

_Closed by @sharkdp on 2025-07-09 08:04_

---
