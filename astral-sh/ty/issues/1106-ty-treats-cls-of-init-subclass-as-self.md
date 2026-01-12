```yaml
number: 1106
title: ty treats cls of __init_subclass__ as self.
type: issue
state: closed
author: danielknell
labels:
  - runtime semantics
assignees: []
created_at: 2025-08-29T23:31:27Z
updated_at: 2025-09-01T08:16:29Z
url: https://github.com/astral-sh/ty/issues/1106
synced_at: 2026-01-12T15:54:24Z
```

# ty treats cls of __init_subclass__ as self.

---

_@danielknell_

### Summary

ty appears to treat the `cls` argument of `__init_subclass__` as `self`, and any changes appear to be interpreted as being applied to the instance rather than the class.

```python
import typing

class Foo:
    def say(self, v) -> None:
        pass

    def __init_subclass__(cls, *args, **kwargs) -> None:
        cls.say = lambda self, v: print(v)

        super().__init_subclass__(*args, **kwargs)

class Bar(Foo):
    pass

bar = Bar()

bar.say("hello world!")

typing.reveal_type(bar.say)
```

yields the following output from ty:

```
No argument provided for required parameter `v` of function `say` (missing-argument) [Ln 17, Col 1]
Revealed type: `(bound method Bar.say(v) -> None) | (def say(self, v) -> None)` (revealed-type) [Ln 19, Col 20]
```

and the following output from python:

```
hello world!
Runtime type is 'method'
```

python properly binds the function and passes `self`, yet ty interpretes it as the function having been assigned to the instance.

adding a `@classmethod` decorator to `__init_subclass__` causes ty to interpret things correctly, but `__init_subclass__` is implicitly a classmethod and is not normally decorated.

Playground for the above:

https://play.ty.dev/9fd17afa-7a28-437b-a459-149c95b85c96

### Version

ty 0.0.1-alpha.19

---

_Comment by @danielknell on 2025-08-30 13:55_

```python
class Foo:
    def __init_subclass__(cls, *args, **kwargs) -> None:
        cls.say = lambda self, v: print(v)

        super().__init_subclass__(*args, **kwargs)

class Bar(Foo):
    pass

typing.reveal_type(Bar.say)
```

also results in 

```
Attribute `say` can only be accessed on instances, not on the class object `<class 'Bar'>` itself.
```

which seems to imply ty is not handling `__init_subclass__` correctly and treats it as an instance method.

---

_Label `runtime semantics` added by @sharkdp on 2025-09-01 07:16_

---

_Assigned to @sharkdp by @sharkdp on 2025-09-01 07:23_

---

_Comment by @sharkdp on 2025-09-01 07:46_

Thank you for reporting this.

https://github.com/astral-sh/ruff/pull/20190 should fix this.

---

_Closed by @sharkdp on 2025-09-01 08:16_

---
