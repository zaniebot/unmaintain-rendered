```yaml
number: 1139
title: "I001: `ruff` and `black` disagree on the number of blank lines after imports in `.pyi`"
type: issue
state: closed
author: mikeroll
labels:
  - bug
assignees: []
created_at: 2022-12-08T11:02:00Z
updated_at: 2022-12-08T16:34:42Z
url: https://github.com/astral-sh/ruff/issues/1139
synced_at: 2026-01-12T15:54:41Z
```

# I001: `ruff` and `black` disagree on the number of blank lines after imports in `.pyi`

---

_@mikeroll_

[Black's](https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html#typing-stub-files)/PEP-484's style guidelines for `pyi` files differ slightly but `ruff` doesn't seem to follow those at least in the following case:

`ruff --fix`:
```pyi
# a_stub.pyi
import decimal


class SomeType:
   var: decimal.Decimal
```

`black` (this results in a I001 from `ruff`):
```pyi
# a_stub.pyi
import decimal

class SomeType:
   var: decimal.Decimal
```

---

_Comment by @charliermarsh on 2022-12-08 14:21_

Oh thank you! I wasn’t aware that Black treated them differently. Will fix.

---

_Label `bug` added by @charliermarsh on 2022-12-08 14:21_

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-08 15:12_

---

_Comment by @charliermarsh on 2022-12-08 15:20_

(This'll go out today.)

---

_Closed by @charliermarsh on 2022-12-08 16:34_

---
