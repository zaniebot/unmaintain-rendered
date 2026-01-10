---
number: 5281
title: F541 deletes fstring w/o adding additional whitespace
type: issue
state: closed
author: jasikpark
labels:
  - bug
assignees: []
created_at: 2023-06-22T02:35:28Z
updated_at: 2023-06-22T21:21:10Z
url: https://github.com/astral-sh/ruff/issues/5281
synced_at: 2026-01-10T01:22:44Z
---

# F541 deletes fstring w/o adding additional whitespace

---

_Issue opened by @jasikpark on 2023-06-22 02:35_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

With this program:

```py
(""f""r"")
g=""f"{'{:f'}"#
```

F541 deletes the `f` in the first fstring and does not add whitespace, producing an invalid doc

```
thread '<unnamed>' panicked at 'Fixed source has a syntax error where the source document does not. This is a bug in one of the generated fixes:
<filename>:2:16: E999 SyntaxError: unexpected EOF while parsing
  |
1 | (""""r"")
2 | g=""f"{'{:f'}"#
  |                 E999
  |


Last generated fixes:
<filename>:1:4: F541 [*] f-string without any placeholders
  |
1 | (""f""r"")
  |    ^^^ F541
2 | g=""f"{'{:f'}"#
  |
  = help: Remove extraneous `f` prefix

ℹ Fix
1   |-(""f""r"")
  1 |+(""""r"")
2 2 | g=""f"{'{:f'}"#


Source with applied fixes:
(""""r"")
g=""f"{'{:f'}"#'
```

found via #4822, on main (1c0a3a467f402676553c63d501aae7455c6c67b0)

---

_Label `bug` added by @charliermarsh on 2023-06-22 02:36_

---

_Comment by @charliermarsh on 2023-06-22 02:37_

Thanks! Looks a bit like #4827.

---

_Comment by @jasikpark on 2023-06-22 02:49_

indeed!

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-22 20:47_

---

_Referenced in [astral-sh/ruff#5319](../../astral-sh/ruff/pulls/5319.md) on 2023-06-22 20:57_

---

_Closed by @charliermarsh on 2023-06-22 21:21_

---
