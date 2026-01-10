---
number: 10945
title: "New check: f-string containing only a single placeholder?"
type: issue
state: open
author: aureliojargas
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-04-15T05:55:10Z
updated_at: 2024-12-05T04:17:11Z
url: https://github.com/astral-sh/ruff/issues/10945
synced_at: 2026-01-10T01:22:50Z
---

# New check: f-string containing only a single placeholder?

---

_Issue opened by @aureliojargas on 2024-04-15 05:55_

Would it be useful to have a check for an f-string whose sole content is a placeholder?

```python
a = "foo"
b = f"{a}"
print(b)
```

This could changed to `b = a` or `b = str(a)`, depending on `a` type.

```console
$ ruff --version
ruff 0.3.7

$ ruff check --isolated --select ALL foo.py
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
foo.py:1:1: D100 Missing docstring in public module
foo.py:3:1: T201 `print` found
Found 2 errors.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
$
```


---

_Label `rule` added by @MichaReiser on 2024-04-15 06:51_

---

_Label `needs-decision` added by @dhruvmanila on 2024-12-05 03:59_

---

_Referenced in [astral-sh/ruff#14780](../../astral-sh/ruff/issues/14780.md) on 2024-12-05 03:59_

---

_Comment by @KotlinIsland on 2024-12-05 04:16_

> This could changed to `b = a` or `b = str(a)`, depending on `a` type.

or `format(a)`, it's actually kinda tricky to statically determine which.

also, should cases like:
```py
a = f"{foo:bar}"
```
be reported in favour of `format(foo, "bar")`

---
