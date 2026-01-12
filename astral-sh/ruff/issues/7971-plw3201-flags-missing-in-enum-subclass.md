```yaml
number: 7971
title: PLW3201 flags _missing_ in Enum subclass
type: issue
state: closed
author: mal
labels:
  - bug
assignees: []
created_at: 2023-10-16T01:27:17Z
updated_at: 2023-10-16T18:11:16Z
url: https://github.com/astral-sh/ruff/issues/7971
synced_at: 2026-01-12T15:54:47Z
```

# PLW3201 flags _missing_ in Enum subclass

---

_@mal_

`Enum` in Python has a few [supported sunder names](https://docs.python.org/3/library/enum.html#supported-sunder-names). Of them, `_missing_` and `_generate_next_value_` are explicitly called out as overridable but when doing so they get flagged as misspelled dunder methods.

Looks separate from, but related to #4592.

```py
from enum import StrEnum
class Build(StrEnum):
    @classmethod
    def _missing_(cls, value):
        pass
```

```sh
$ ruff --isolated --preview --select PLW3201 --show-source test.py
test.py:4:9: PLW3201 Bad or misspelled dunder method name `_missing_`. (bad-dunder-name)
  |
2 | class Build(StrEnum):
3 |     @classmethod
4 |     def _missing_(cls, value):
  |         ^^^^^^^^^ PLW3201
5 |         pass
  |
```

Observed using `ruff 0.0.292`.


---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-16 16:39_

---

_Label `bug` added by @charliermarsh on 2023-10-16 16:39_

---

_Comment by @charliermarsh on 2023-10-16 16:42_

Thanks!

---

_Closed by @charliermarsh on 2023-10-16 18:11_

---
