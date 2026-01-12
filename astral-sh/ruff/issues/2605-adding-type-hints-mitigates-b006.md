```yaml
number: 2605
title: Adding type hints mitigates B006
type: issue
state: closed
author: luator
labels:
  - question
assignees: []
created_at: 2023-02-06T11:42:14Z
updated_at: 2023-02-06T19:28:10Z
url: https://github.com/astral-sh/ruff/issues/2605
synced_at: 2026-01-12T15:54:43Z
```

# Adding type hints mitigates B006

---

_@luator_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

I noticed that the B006 warning (using mutable data structure as argument default value) disappears when I add a type hint for the argument.  Minimal example:
```python
from typing import Sequence


class Foo:
    def foo(self, x: Sequence[int] = [1, 2, 3]):  # no warning here
        ...

    def bar(self, x=[1, 2, 3]):  # B006
        ...
```
Output of `ruff check --isolated --select B foo.py`:
```
/tmp/foo.py:8:21: B006 Do not use mutable data structures for argument defaults
Found 1 error.
```

As the type hint doesn't change the behaviour, there should be a B006 warning in both cases.  I quickly double-checked and flake8 indeed gives a warning for both methods.

Ruff version: 0.0.241

---

_Comment by @charliermarsh on 2023-02-06 15:45_

This is actually an intentional change -- if you use a type annotation with an immutable type, we don't warn, since your type checker should tell you that you can't modify these:

```rust
const IMMUTABLE_GENERIC_TYPES: &[&[&str]] = &[
    &["", "tuple"],
    &["collections", "abc", "ByteString"],
    &["collections", "abc", "Collection"],
    &["collections", "abc", "Container"],
    &["collections", "abc", "Iterable"],
    &["collections", "abc", "Mapping"],
    &["collections", "abc", "Reversible"],
    &["collections", "abc", "Sequence"],
    &["collections", "abc", "Set"],
    &["typing", "AbstractSet"],
    &["typing", "ByteString"],
    &["typing", "Callable"],
    &["typing", "Collection"],
    &["typing", "Container"],
    &["typing", "FrozenSet"],
    &["typing", "Iterable"],
    &["typing", "Literal"],
    &["typing", "Mapping"],
    &["typing", "Never"],
    &["typing", "NoReturn"],
    &["typing", "Reversible"],
    &["typing", "Sequence"],
    &["typing", "Tuple"],
];
```

We could consider making this configurable, but it is working as intended.


---

_Label `question` added by @charliermarsh on 2023-02-06 15:45_

---

_Comment by @luator on 2023-02-06 16:56_

Oh, I see.  I have to admit, I wasn't aware that `typing.Sequence` is explicitly denoting an immutable type.  Indeed, if change the annotation to `MutableSequence`, I get the B006 in both cases.

While I understand the reasoning, I would vote to indeed make this configurable.  The current behaviour relies on 1) me using additional tools (which I may not always do) and 2) the code being fully annotated.  For example the following code does not result in a mypy error because `func()` is not annotated and thus not checked.
```python
from typing import Sequence

class Foo:
    def __init__(self, x: Sequence[int] = [1, 2, 3]):
        self.x = x

def func(f):
    f.x[0] = 42

f1 = Foo()
func(f1)

f2 = Foo()
print(f2.x)  # prints [42, 2, 3]
```

That said, I could also understand if you don't want to bloat ruff with too many fine-grained settings.

---

_Comment by @charliermarsh on 2023-02-06 19:28_

Yeah I think my preference here is to leave the existing behavior as-is. I understand why one might want to disable this, but I think the current behavior is reasonable and I'd rather keep the bar a little higher for adding more settings.

---

_Closed by @charliermarsh on 2023-02-06 19:28_

---
