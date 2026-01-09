---
number: 16922
title: PyUpgrade rule UP007 should be disabled on older Pythons
type: issue
state: closed
author: cameron-simpson
labels:
  - needs-info
assignees: []
created_at: 2025-03-22T22:49:53Z
updated_at: 2025-03-29T02:07:32Z
url: https://github.com/astral-sh/ruff/issues/16922
synced_at: 2026-01-07T13:12:16-06:00
---

# PyUpgrade rule UP007 should be disabled on older Pythons

---

_Issue opened by @cameron-simpson on 2025-03-22 22:49_

### Summary

I'm not sure when general X|Y types became allowed, but in Python 3.8 `int|str` and similar elicits a `TypeError`. I've got my `.ruff.toml` `target-version` set to `"py38"` and that should disable this rule.


---

_Comment by @cameron-simpson on 2025-03-22 22:51_

I suspect that I'm suggesting: PyUpgrade rules shouldn't suggest upgrades beyond the `target-version` if one is set.


---

_Referenced in [lusoul232/docs#1](../../lusoul232/docs/issues/1.md) on 2025-03-22 23:46_

---

_Comment by @InSyncWithFoo on 2025-03-23 03:21_

> PyUpgrade rules shouldn't suggest upgrades beyond the `target-version` if one is set.

This is already the case:

```shell
$ ruff check --isolated --select UP007 --target-version py39 -
from typing import Union
a: Union[str, int]
All checks passed!
```

```shell
$ ruff check --isolated --select UP007 --target-version py310 -
from typing import Union
a: Union[str, int]
-:2:4: UP007 [*] Use `X | Y` for type annotations
  |
1 | from typing import Union
2 | a: Union[str, int]
  |    ^^^^^^^^^^^^^^^ UP007
  |
  = help: Convert to `X | Y`

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

What do your configurations and code look like? Please provide a minimal reproducible example.

---

_Label `needs-info` added by @MichaReiser on 2025-03-23 07:58_

---

_Comment by @eli-schwartz on 2025-03-23 08:43_

From the UP007 docs:

> This rule is enabled when targeting Python 3.10 or later (see: [`target-version`](https://docs.astral.sh/ruff/settings/#target-version)). By default, it's *also* enabled for earlier Python versions if `from __future__ import annotations` is present, as `__future__` annotations are not evaluated at runtime. If your code relies on runtime type annotations (either directly or via a library like Pydantic), you can disable this behavior for Python versions prior to 3.10 by setting [`lint.pyupgrade.keep-runtime-typing`](https://docs.astral.sh/ruff/settings/#lint_pyupgrade_keep-runtime-typing) to `true`.

I suspect the answer to this ticket is "you are seeing expected behavior, but it's a config problem".

---

_Closed by @MichaReiser on 2025-03-27 12:14_

---

_Comment by @cameron-simpson on 2025-03-29 01:27_

@MichaReiser sorry, I've been busy elsewhere.

You're right, it was misconfiguration. My `.ruff.toml` was correct, but overridden by my lint script which had `--target-version=py313` buried in its incantation (for a different piece of code).

I've verified that hand running `ruff check` directly without that option honours the `.ruff.toml` setting.

Sorry to have troubled you.


---

_Comment by @MichaReiser on 2025-03-29 02:07_

No worries. I'm glad you figured it out 

---
