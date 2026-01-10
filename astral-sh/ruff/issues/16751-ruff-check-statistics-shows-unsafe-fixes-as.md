```yaml
number: 16751
title: "`ruff check --statistics` shows unsafe fixes as fixable"
type: issue
state: closed
author: ZedThree
labels:
  - cli
assignees: []
created_at: 2025-03-14T15:41:57Z
updated_at: 2025-03-18T07:03:15Z
url: https://github.com/astral-sh/ruff/issues/16751
synced_at: 2026-01-10T11:09:57Z
```

# `ruff check --statistics` shows unsafe fixes as fixable

---

_Issue opened by @ZedThree on 2025-03-14 15:41_

### Summary

Take the following simple example:

```py
def mvce(keys, values):
    return {key: value for key, value in zip(keys, values)}
```

and run: `ruff check --select=C416 mvce.py`:

```
mvce.py:2:12: C416 Unnecessary dict comprehension (rewrite using `dict()`)
  |
1 | def mvce(keys, values):
2 |     return {key: value for key, value in zip(keys, values)}
  |            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ C416
  |
  = help: Rewrite using `dict()`

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

but running `ruff check --select=C416 --statistics mvce.py`:

```
1       C416    [*] unnecessary-comprehension
[*] fixable with `ruff check --fix`
```

Is this intentional? There _is_ a fix available, but it's unsafe, so at the very least the summary line for `--statistics` is inaccurate in that it also requires `--unsafe-fixes`.

I have a simple fix to only count unsafe fixes if `--unsafe-fixes` is set, and can make a PR if this isn't intentional behaviour.

In either case, should the summary be updated to mention unsafe fixes?

### Version

ruff 0.11.0+2 (631141237 2025-03-14)

---

_Label `cli` added by @ntBre on 2025-03-14 15:47_

---

_Comment by @ntBre on 2025-03-14 15:57_

This sounds unintentional to me, especially because `Printer::write_once` appears to check the fix applicability (via `FixableStatistics`), unlike `Printer::write_statistics`, but @MichaReiser or @dhruvmanila would know better than me.

---

_Closed by @MichaReiser on 2025-03-18 07:03_

---

_Closed by @MichaReiser on 2025-03-18 07:03_

---
