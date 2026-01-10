```yaml
number: 13137
title: RUF012 does not catch mutable default in dataclass
type: issue
state: open
author: msehnout
labels:
  - type-inference
assignees: []
created_at: 2024-08-28T10:52:19Z
updated_at: 2025-01-10T15:44:07Z
url: https://github.com/astral-sh/ruff/issues/13137
synced_at: 2026-01-10T11:09:55Z
```

# RUF012 does not catch mutable default in dataclass

---

_Issue opened by @msehnout on 2024-08-28 10:52_

Hello,

I stumbled upon this bug in my code which I thought ruff should catch: 
```python
from dataclasses import dataclass


class Foo:
    def __init__(self) -> None:
        self.counter: int = 0


@dataclass
class Bar:
    foo: Foo = Foo()


bar1 = Bar()
bar2 = Bar()
bar1.foo.counter = 1

print(bar1.foo.counter)
print(bar2.foo.counter)
```

It is fairly typical example of the https://docs.astral.sh/ruff/rules/mutable-class-default/ bug. Once I found the root cause, it was obvious how to fix it, but I expected `ruff` to tell me.

Result:
```
$ ruff check main.py 
All checks passed!
$ ruff version
ruff 0.6.2
$ python3 main.py
1
1
```

Would it be possible to handle this case?

Thanks for your response and for creating `ruff`, it is awesome!
Martin

---

_Comment by @AlexWaygood on 2024-08-28 12:06_

I think it's hard to do much better here without more sophisticated type inference (which is a long-term goal, but which is unlikely to happen for a while).

RUF012 decides whether a class assignment could have a mutable default [here](https://github.com/astral-sh/ruff/blob/cfafaa7637ef3f568d62d39654e0498f52a07b77/crates/ruff_linter/src/rules/ruff/rules/mutable_class_default.rs#L56-L93). For call expressions (like `Foo()`), it defers to this function [here](https://github.com/astral-sh/ruff/blob/cfafaa7637ef3f568d62d39654e0498f52a07b77/crates/ruff_python_semantic/src/analyze/typing.rs#L291-L297) to decide whether the result of the call expression might be a mutable object -- and that function uses [this function](https://github.com/astral-sh/ruff/blob/cfafaa7637ef3f568d62d39654e0498f52a07b77/crates/ruff_python_stdlib/src/typing.rs#L284-L295) to compare the qualified name of the call expression against a list of Python functions that are known to create mutable objects when they're called.

With generalised type inference capabilities, we'd be able to do a much better job here -- we could lookup the class `Foo`, and analyse the class to see if it has any mutable attributes; if so, we'd know that `Foo()` creates a mutable object. We could _potentially_ do this now for classes that are located in the same file, but I think it would add a _lot_ of complexity, and we wouldn't be able to follow imports to other files. Without doing that kind of complicated analysis, it's hard for me to see how we could otherwise catch this without introducing lots of false-positive errors.

---

_Label `type-inference` added by @AlexWaygood on 2024-08-28 12:06_

---

_Comment by @msehnout on 2024-08-28 13:05_

Ok :-) I was afraid that this would be the answer, but it makes sense that deciding whether a class is mutable or not is a difficult question in Python.

---

_Comment by @beauxq on 2025-01-10 15:28_

For function parameter defaults, B008 would catch this.
Couldn't that same logic be used to catch this?

I think it would be good for class member defaults to follow the same rules as function parameter defaults. Otherwise, it seems like an inconsistency.

---
