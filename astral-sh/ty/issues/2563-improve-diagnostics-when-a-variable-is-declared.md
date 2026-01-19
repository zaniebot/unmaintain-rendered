```yaml
number: 2563
title: "Improve diagnostics when a variable is declared to have a type from the `numbers` module and an `int`/`float`/`complex` is passed"
type: issue
state: open
author: tubarao312
labels:
  - diagnostics
assignees: []
created_at: 2026-01-19T15:00:56Z
updated_at: 2026-01-19T15:06:28Z
url: https://github.com/astral-sh/ty/issues/2563
synced_at: 2026-01-19T15:24:48Z
```

# Improve diagnostics when a variable is declared to have a type from the `numbers` module and an `int`/`float`/`complex` is passed

---

_@tubarao312_

### Summary

Integer integers are *not* considered `numbers.Number`. Also appears to happen for `float` and `complex` numbers.

```py
from numbers import Number

a: Number = 1
#           ~
# Object of type `Literal[1]` is not assignable to `Number` (invalid-assignment)

```

[Playground Link](https://play.ty.dev/5e142380-4474-46fa-841e-6fa871b0ae7c)

### Version

ty 0.0.12

---

_Label `diagnostics` added by @AlexWaygood on 2026-01-19 15:02_

---

_Comment by @AlexWaygood on 2026-01-19 15:05_

Thanks for the report. All other type checkers agree with ty here, and it would be unsound for ty to do anything else here, unfortunately. For more information, you can see https://discuss.python.org/t/numeric-generics-where-do-we-go-from-pep-3141-and-present-day-mypy/17155, https://github.com/python/mypy/issues/3186, and https://stackoverflow.com/questions/69334475/how-to-hint-at-number-types-i-e-subclasses-of-number-not-numbers-themselv.

However, one thing we definitely should do here is improve our diagnostic for these situations. Mypy has a special-cased diagnostic specifically for this issue because it's so common; we should do something similar. It doesn't look like it emits the special diagnostic on your snippet specifically, but on this snippet:

```py
from numbers import Number

def f(x: Number): ...

f(1)
```

it emits this message, which is very nice:

```
main.py:5: error: Argument 1 to "f" has incompatible type "int"; expected "Number"  [arg-type]
main.py:5: note: Types from "numbers" aren't supported for static type checking
main.py:5: note: See https://peps.python.org/pep-0484/#the-numeric-tower
main.py:5: note: Consider using a protocol instead, such as typing.SupportsFloat
```



---

_Renamed from "Object of type `Literal[1]` is not assignable to `numbers.Number`" to "Improve diagnostics when a variable is declared to have a type from the `numbers` module and an `int`/`float`/`complex` is passed" by @AlexWaygood on 2026-01-19 15:06_

---
