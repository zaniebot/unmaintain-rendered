```yaml
number: 6409
title: flake8-tkinter plugin
type: issue
state: open
author: benjamin-kirkbride
labels:
  - plugin
  - needs-decision
assignees: []
created_at: 2023-08-08T00:07:15Z
updated_at: 2023-08-13T19:21:09Z
url: https://github.com/astral-sh/ruff/issues/6409
synced_at: 2026-01-12T15:54:46Z
```

# flake8-tkinter plugin

---

_@benjamin-kirkbride_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Would be great to have this added to the list of rules :)

https://pypi.org/project/flake8-tkinter/

---

_Label `plugin` added by @charliermarsh on 2023-08-08 18:46_

---

_Label `needs-decision` added by @charliermarsh on 2023-08-08 18:46_

---

_Comment by @rdbende on 2023-08-13 19:21_

Hi, author of `flake8-tkinter` here!

Unfortunately I can't help implementing this, but I would love to see it in Ruff 😍
I have a few projects where we use both Ruff for its speed and flake8 because I need the extra tkinter-related checks. It may come in handy for other projects as well.

To help prevent false-positives, `flake8-tkinter` checks for tkinter imports and only runs if there are any. There are currently no known false positives, however there are important checks missing for well-known but common mistakes and best practices.

---
