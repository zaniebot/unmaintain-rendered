```yaml
number: 3065
title: "[N999] `__version__.py` flagged as false positive (Ruff 0.0.248)"
type: issue
state: closed
author: eddiebergman
labels: []
assignees: []
created_at: 2023-02-20T18:53:24Z
updated_at: 2023-02-20T19:27:15Z
url: https://github.com/astral-sh/ruff/issues/3065
synced_at: 2026-01-12T15:54:43Z
```

# [N999] `__version__.py` flagged as false positive (Ruff 0.0.248)

---

_@eddiebergman_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
The rule N999 (snake_case module names) seems to trigger on `__version__.py` which seems incorrect based on the pep8 rule linked in the docs.

If you think it's correct however, feel free to close :)

Keep on truckin'


---

_Comment by @charliermarsh on 2023-02-20 18:55_

I think this should be fixed in v0.0.249, which is building now (and was released early for this purpose :))

If you see it on v0.0.249, definitely let me know!

---

_Closed by @charliermarsh on 2023-02-20 18:55_

---

_Comment by @eddiebergman on 2023-02-20 18:56_

Wow, your response was like ruff, blazingly fast. Amazing work, thank you!

---

_Comment by @eddiebergman on 2023-02-20 19:27_

Fixes the problem, thank you!

---
