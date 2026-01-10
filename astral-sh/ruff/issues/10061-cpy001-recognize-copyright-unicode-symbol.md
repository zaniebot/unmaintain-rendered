---
number: 10061
title: "CPY001: recognize copyright unicode symbol"
type: issue
state: closed
author: nijel
labels:
  - bug
assignees: []
created_at: 2024-02-20T12:46:10Z
updated_at: 2024-02-22T17:44:24Z
url: https://github.com/astral-sh/ruff/issues/10061
synced_at: 2026-01-10T01:22:49Z
---

# CPY001: recognize copyright unicode symbol

---

_Issue opened by @nijel on 2024-02-20 12:46_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Following snippet:

```python
# Copyright © 2024 Michal Čihař <michal@weblate.org>
```

Fails:

```console
$ ruff check --preview weblate/wsgi.py 
weblate/wsgi.py:1:1: CPY001 Missing copyright notice at top of file
  |
1 | # Copyright © 2024 Michal Čihař <michal@weblate.org>
  |  CPY001
```

It doesn't understand `©`, it only looks for `(c)`.

---

_Label `bug` added by @charliermarsh on 2024-02-20 16:27_

---

_Comment by @charliermarsh on 2024-02-20 16:27_

Makes sense - thanks!

---

_Referenced in [astral-sh/ruff#10065](../../astral-sh/ruff/pulls/10065.md) on 2024-02-20 16:39_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-20 16:40_

---

_Closed by @charliermarsh on 2024-02-22 17:44_

---
