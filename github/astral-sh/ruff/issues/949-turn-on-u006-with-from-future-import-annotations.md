---
number: 949
title: "Turn on U006 with 'from __future__ import annotations'"
type: issue
state: closed
author: brandon-leapyear
labels: []
assignees: []
created_at: 2022-11-28T23:23:13Z
updated_at: 2024-02-26T06:07:06Z
url: https://github.com/astral-sh/ruff/issues/949
synced_at: 2026-01-07T13:12:14-06:00
---

# Turn on U006 with 'from __future__ import annotations'

---

_Issue opened by @brandon-leapyear on 2022-11-28 23:23_

It seems like if `target-version` is below 3.9, U006 doesn't get triggered _at all_, even if `from __future__ import annotations` is included:

https://github.com/charliermarsh/ruff/blob/731fba90060de58f9893251d62913b9294fa2458/src/check_ast.rs#L1231-L1236

Repro:
```py
from __future__ import annotations

from typing import Optional

def foo(x: int) -> Optional[int]:
  return None
```
```console
$ ruff --select U foo.py
Found 1 error(s).
foo.py:5:20: U007 Use `X | Y` for type annotations
1 potentially fixable with the --fix option.

$ ruff --target-version=py38 --select U foo.py
```

Related: https://github.com/charliermarsh/ruff/issues/778

---

_Comment by @charliermarsh on 2022-11-29 01:00_

Interesting, I honestly had no idea that `from __future__ import annotations` enabled this behavior for pre-3.9 versions (https://peps.python.org/pep-0585/#implementation). Will fix.

---

_Label `enhancement` added by @charliermarsh on 2022-11-29 01:00_

---

_Comment by @brandon-leapyear on 2022-11-29 01:01_

:sparkles: _**This is an old work account. Please reference @brandonchinn178 for all future communication**_ :sparkles:
<!-- updated by mention_personal_account_in_comments.py -->

---

To be extra clear, this only affects _annotations_. Anything at runtime should _not_ be converted pre-3.9

---

_Assigned to @charliermarsh by @charliermarsh on 2022-11-29 01:33_

---

_Referenced in [astral-sh/ruff#953](../../astral-sh/ruff/pulls/953.md) on 2022-11-29 01:55_

---

_Closed by @charliermarsh on 2022-11-29 01:57_

---

_Comment by @charliermarsh on 2022-11-29 01:58_

This is going out in [v0.0.145](https://github.com/charliermarsh/ruff/releases/tag/v0.0.145).

---

_Comment by @tmke8 on 2022-12-01 15:38_

It seems that in the new release, type aliases also trigger the check. Like in

```python
from __future__ import annotations
from typing import List, Union
from typing_extensions import TypeAlias

MyList: TypeAlias = Union[List[int], List[str]]
```

ruff output:
```
Found 3 error(s).
main.py:5:21: U007 Use `X | Y` for type annotations
main.py:5:27: U006 Use `list` instead of `List` for type annotations
main.py:5:38: U006 Use `list` instead of `List` for type annotations
3 potentially fixable with the --fix option.
```

However, if I went along with the suggested fix, I would get runtime errors.

---

_Comment by @charliermarsh on 2022-12-01 15:40_

Taking a look...

---

_Comment by @charliermarsh on 2022-12-01 15:48_

Okay I think this is going to require a more thoughtful fix. I see the issue but need to differentiate between a few cases in the code.

You should be able to disable this for now with:

```toml
[tool.ruff.pyupgrade]
# Preserve types, even if a file imports `from __future__ import annotations`.
keep-runtime-typing = true
```

---

_Referenced in [astral-sh/ruff#979](../../astral-sh/ruff/issues/979.md) on 2022-12-01 15:49_

---

_Comment by @charliermarsh on 2022-12-01 15:53_

(I'll try to fix this today, gotta get to a few other things first! Now tracking in #979.)

---
