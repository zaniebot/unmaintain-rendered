```yaml
number: 9831
title: Support formatting using line ranges
type: issue
state: closed
author: jvacek
labels: []
assignees: []
created_at: 2024-02-05T13:37:34Z
updated_at: 2024-02-05T13:38:51Z
url: https://github.com/astral-sh/ruff/issues/9831
synced_at: 2026-01-12T15:54:49Z
```

# Support formatting using line ranges

---

_@jvacek_

Black added support for formatting line-ranges https://github.com/psf/black/pull/4020

Would be nice if this was possible within ruff's formatter. The Black VSCode extension also has support for this now, and allows code to be formatted "on modification" https://github.com/microsoft/vscode-black-formatter/pull/380
<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Comment by @MichaReiser on 2024-02-05 13:38_

Hy @jvacek Good news. This should be supported in the next ruff release https://github.com/astral-sh/ruff/pull/9733

---

_Closed by @MichaReiser on 2024-02-05 13:38_

---
