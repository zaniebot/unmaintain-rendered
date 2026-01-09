---
number: 2406
title: F841 unused-variable false negative?
type: issue
state: closed
author: spaceone
labels: []
assignees: []
created_at: 2023-01-31T18:29:04Z
updated_at: 2023-01-31T19:04:36Z
url: https://github.com/astral-sh/ruff/issues/2406
synced_at: 2026-01-07T13:12:14-06:00
---

# F841 unused-variable false negative?

---

_Issue opened by @spaceone on 2023-01-31 18:29_

```sh
$ cat foo.py                                                                                                       
class TestFoo(object):
    def test(self, tmpdir):
        _test = []
        assert tmpdir$ flake8 foo.py
foo.py:3:9: F841 local variable '_test' is assigned to but never used
$ ruff --isolated --select F841 foo.py
```

variables starting with `_` are not detected as unused in `ruff` but in `flake8`.
I detected this as prior it had a `# noqa: F841` comment which was removed by `RUF100` when I compared any leftovers with `flake8`.

---

_Comment by @charliermarsh on 2023-01-31 19:04_

This is intentional! You should be able to change [`dummy-variable-rgx`](https://github.com/charliermarsh/ruff#dummy-variable-rgx) to ignore this.

---

_Closed by @charliermarsh on 2023-01-31 19:04_

---
