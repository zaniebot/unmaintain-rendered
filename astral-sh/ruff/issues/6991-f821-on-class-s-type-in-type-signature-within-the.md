```yaml
number: 6991
title: "F821 On class's type in type signature within the same class."
type: issue
state: closed
author: 0x6d6e647a
labels:
  - question
assignees: []
created_at: 2023-08-29T18:28:40Z
updated_at: 2023-08-29T18:57:43Z
url: https://github.com/astral-sh/ruff/issues/6991
synced_at: 2026-01-10T11:09:49Z
```

# F821 On class's type in type signature within the same class.

---

_Issue opened by @0x6d6e647a on 2023-08-29 18:28_

I'm using Ruff v0.0.286 in the following.

For the following Python code:
```python
#!/usr/bin/env python

class Foo:
    def __init__(self, name: str) -> None:
        self._name = str

    @staticmethod
    def from_int(n: int) -> Foo:
        return Foo(str(n))

x = Foo.from_int(5)
``` 
Ruff generates the following error:
```shell
user@host $ ruff test.py
test.py:8:29: F821 Undefined name `Foo`
Found 1 error.
``` 

Whereas Mypy says there is no error:
```shell
user@host $ mypy test.py
Success: no issues found in 1 source file
```

It seems that Ruff is not aware of a class's type definition from within the same class but only in side a type signature.

Please note that Ruff **correctly** does not error on the next line `return Foo(str(n))`. The issue seems to only happen within type signatures.
<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Comment by @zanieb on 2023-08-29 18:46_

Hi!

I believe our behavior is correct here, the class is _not_ available in the method signature scope:

```
❯ python example.py
Traceback (most recent call last):
  File "/Users/mz/eng/src/astral-sh/ruff/example.py", line 3, in <module>
    class Foo:
  File "/Users/mz/eng/src/astral-sh/ruff/example.py", line 8, in Foo
    def from_int(n: int) -> Foo:
                            ^^^
NameError: name 'Foo' is not defined
```

---

_Label `question` added by @zanieb on 2023-08-29 18:47_

---

_Comment by @0x6d6e647a on 2023-08-29 18:56_

You're correct! I fixed the code as follows:

```python
#!/usr/bin/env python
from typing import Self

class Foo:
    def __init__(self, name: str) -> None:
        self._name = str

    @classmethod
    def from_int(cls, n: int) -> Self:
        return cls(str(n))

x = Foo.from_int(5)
```

Sorry for wasting your time. I really appreciate the quick reply! Thank you very much!

---

_Closed by @0x6d6e647a on 2023-08-29 18:56_

---

_Comment by @zanieb on 2023-08-29 18:57_

No problem! You can also use `"Foo"` but `Self` is a great new feature.

---
