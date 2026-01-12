```yaml
number: 2468
title: typing.Self is not recognized as compatible with self
type: issue
state: closed
author: lelit
labels: []
assignees: []
created_at: 2026-01-12T14:44:52Z
updated_at: 2026-01-12T15:07:16Z
url: https://github.com/astral-sh/ty/issues/2468
synced_at: 2026-01-12T15:54:26Z
```

# typing.Self is not recognized as compatible with self

---

_@lelit_

### Summary

This is similar to #2255, but triggers a different violation, so I opted to open a new issue.

The following is a stripped down version of a _mixin_  I usually add to my `ORM` classes:

```python
from __future__ import annotations

from collections.abc import Callable
from collections.abc import Sequence
from typing import Any
from typing import Self


class ReprMixin:
    REPR_ATTRS: Sequence[tuple[str, Callable[[Self, str], Any]] | str] | None = None

    def __repr__(self) -> str:
        classname = self.__class__.__name__
        if self.REPR_ATTRS is not None:
            attrs = []
            for item in self.REPR_ATTRS:
                if isinstance(item, str):
                    attrs.append((item, getattr(self, item)))
                else:
                    name, func  = item
                    attrs.append((name, func(self, name)))
            attrs_v = ' '.join(f'{n}={v}' for n, v in attrs)
            return f'<{classname} {attrs_v}>'
        else:
            return 'bad configuration'


class Foo(ReprMixin):
    REPR_ATTRS = ('foo', ('bar', lambda s, n: getattr(s, n).upper()))

    def __init__(self):
        self.foo = 'foo'
        self.bar = 'bar'


foo = Foo()
print(repr(foo))
# <Foo foo=foo bar=BAR>
```

This is what I get with `ty check`:
```
$ ty version
ty 0.0.10

$ ty check test.py
error[invalid-argument-type]: Argument is incorrect
  --> test.py:21:46
   |
19 |                 else:
20 |                     name, func  = item
21 |                     attrs.append((name, func(self, name)))
   |                                              ^^^^ Expected `<special-form 'typing.Self'>`, found `Self@__repr__`
22 |             attrs_v = ' '.join(f'{n}={v}' for n, v in attrs)
23 |             return f'<{classname} {attrs_v}>'
   |
info: rule `invalid-argument-type` is enabled by default

Found 1 diagnostic
```

For the record, also ``mypy` is not happy:
```
$ mypy --version
mypy 1.17.1 (compiled: yes)

$ mypy test.py 
test.py:21: error: Argument 1 has incompatible type "ReprMixin"; expected "Self"  [arg-type]
Found 1 error in 1 file (checked 1 source file)
```

OTOH, `pyrefly` likes it:
```
$ pyrefly --version
pyrefly 0.47.0

$ pyrefly check test.py 
 INFO 0 errors
```

and so does `pyright`:
```
$ pyright --version
pyright 1.1.407

$ pyright test.py 
0 errors, 0 warnings, 0 informations
```

See also the corresponding [playground](https://play.ty.dev/4b803ba2-7fe3-4be2-8649-19f7ffc1402d).


### Version

0.0.10

---

_Comment by @AlexWaygood on 2026-01-12 14:56_

I think this is https://github.com/astral-sh/ty/issues/1124

---

_Comment by @lelit on 2026-01-12 15:06_

Yes, it seems so, sorry about not spotting it, distracted by the "dataclass" in the title 🤷 

---

_Comment by @AlexWaygood on 2026-01-12 15:07_

No problem, GitHub issue search is anyway impossible to navigate :-)

---

_Closed by @AlexWaygood on 2026-01-12 15:07_

---
