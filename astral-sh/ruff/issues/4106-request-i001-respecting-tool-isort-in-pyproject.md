```yaml
number: 4106
title: "Request: `I001` respecting `[tool.isort]` in `pyproject.toml`"
type: issue
state: closed
author: jamesbraza
labels:
  - question
assignees: []
created_at: 2023-04-26T00:03:41Z
updated_at: 2023-04-26T00:49:24Z
url: https://github.com/astral-sh/ruff/issues/4106
synced_at: 2026-01-12T15:54:44Z
```

# Request: `I001` respecting `[tool.isort]` in `pyproject.toml`

---

_@jamesbraza_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

I have a `[tool.isort]` section in a `pyproject.toml` that configures `isort` with `known_first_party` and `known_local_folder`.

I am migrating from `isort` (amongst other tools) to `ruff`.  It would be cool if `ruff` either:
- Respected `[tool.isort]` config section
- Supported a `[tool.ruff.isort]` config section

---

_Comment by @charliermarsh on 2023-04-26 00:06_

We do support `[tool.ruff.isort]`! It supports a lot of the same configuration options -- you can see some examples here: https://beta.ruff.rs/docs/settings/#isort.

---

_Label `question` added by @charliermarsh on 2023-04-26 00:06_

---

_Comment by @charliermarsh on 2023-04-26 00:49_

(Closing but let me know if I'm misunderstanding your issue!)

---

_Closed by @charliermarsh on 2023-04-26 00:49_

---
