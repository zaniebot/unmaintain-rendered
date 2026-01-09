---
number: 10761
title: SyntaxError after bad fix in f-string from Q000 flake8 bad-quotes-inline-string
type: issue
state: closed
author: Hnasar
labels:
  - bug
assignees: []
created_at: 2024-04-03T19:23:41Z
updated_at: 2024-04-04T03:38:50Z
url: https://github.com/astral-sh/ruff/issues/10761
synced_at: 2026-01-07T13:12:15-06:00
---

# SyntaxError after bad fix in f-string from Q000 flake8 bad-quotes-inline-string

---

_Issue opened by @Hnasar on 2024-04-03 19:23_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Using ruff 0.3.5: the fix for https://docs.astral.sh/ruff/rules/bad-quotes-inline-string/ results in a syntax error  

```python
# foo.py
x = y = z = 5
s = f"Before {f'x {x}' if y else f'foo {z}'} after"
```

```console
$ ruff check --target-version=py310 --select=Q foo.py  
foo.py:2:34: Q000 [*] Single quotes found but double quotes preferred
```

```diff
--- foo.py
+++ foo.py
@@ -1,2 +1,2 @@
 x = y = z = 5
-s = f"Before {f'x {x}' if y else f'foo {z}'} after"
+s = f"Before {f'x {x}' if y else f"foo {z}"} after"
```

After applying `--fix`

```console
$ python3.10 foo.py 
  File "foo.py", line 2
    s = f"Before {f'x {x}' if y else f"foo {z}"} after"
                                       ^^^
SyntaxError: f-string: expecting '}'
```

### Notes:
* The "fixed" syntax is only valid in 3.12 because it supports arbitrarily nested fstrings
* I cannot reproduce this bad fix using ruff 0.1.6

---

_Label `bug` added by @charliermarsh on 2024-04-03 23:25_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-04-04 02:45_

---

_Comment by @dhruvmanila on 2024-04-04 02:47_

Oops, sorry. I think I have a permanent fix for this.

---

_Referenced in [astral-sh/ruff#10766](../../astral-sh/ruff/pulls/10766.md) on 2024-04-04 02:53_

---

_Closed by @dhruvmanila on 2024-04-04 03:38_

---

_Closed by @dhruvmanila on 2024-04-04 03:38_

---
