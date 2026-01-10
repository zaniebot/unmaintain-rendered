```yaml
number: 3299
title: "TCH002 false positive for certain target_version or no `__future__.annotations` import"
type: issue
state: closed
author: spaceone
labels:
  - bug
assignees: []
created_at: 2023-03-02T09:46:44Z
updated_at: 2023-03-02T21:45:28Z
url: https://github.com/astral-sh/ruff/issues/3299
synced_at: 2026-01-10T11:09:46Z
```

# TCH002 false positive for certain target_version or no `__future__.annotations` import

---

_Issue opened by @spaceone on 2023-03-02 09:46_

I fixed my code according to `TCH002` and got a `NameError` exception when executing it and the code doesn't have the `from __future__ import annotations` import.

```python
#!/usr/bin/python3

#from __future__ import annotations

from typing import Iterator, TYPE_CHECKING

from apt_pkg import Version

if TYPE_CHECKING:
    from apt_pkg import Version2

class Foo:

    def bar(self) -> Iterator[Version]:
        pkg = self.apt['foo']
        cand = pkg.candidate
        yield cand

    def baz(self) -> Iterator[Version2]:
        pkg = self.apt['foo']
        cand = pkg.candidate
        yield cand

Foo().bar()
Foo().baz()

```
→
```console
$ python3.9 foo.py
Traceback (most recent call last):
  File "foo.py", line 12, in <module>
    class Foo:
  File "foo.py", line 19, in Foo
    def baz(self) -> Iterator[Version2]:
NameError: name 'Version2' is not defined
```


---

_Label `bug` added by @charliermarsh on 2023-03-02 15:42_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-02 18:19_

---

_Comment by @charliermarsh on 2023-03-02 18:20_

👍 Those should be treated as runtime annotations, since they're defined within a class scope. Thank you!

---

_Closed by @charliermarsh on 2023-03-02 21:45_

---
