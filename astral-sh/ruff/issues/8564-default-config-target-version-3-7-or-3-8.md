```yaml
number: 8564
title: Default config target-version 3.7 or 3.8
type: issue
state: closed
author: doolio
labels: []
assignees: []
created_at: 2023-11-08T15:07:01Z
updated_at: 2023-11-08T15:09:35Z
url: https://github.com/astral-sh/ruff/issues/8564
synced_at: 2026-01-12T15:54:48Z
```

# Default config target-version 3.7 or 3.8

---

_@doolio_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

The docs [here](https://docs.astral.sh/ruff/configuration/) state the default target version is Python 3.8 yet the README suggests ruff supports back to 3.7. Should these two be aligned?

```toml
# Assume Python 3.8
target-version = "py38"
```

---

_Comment by @charliermarsh on 2023-11-08 15:08_

@doolio - I believe this is intended as Python 3.7 is EOL, so we no longer default to it.

---

_Comment by @doolio on 2023-11-08 15:09_

I see. OK, sorry for the noise.

---

_Closed by @doolio on 2023-11-08 15:09_

---
