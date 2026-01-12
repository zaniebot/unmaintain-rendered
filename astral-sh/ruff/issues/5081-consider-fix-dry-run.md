```yaml
number: 5081
title: "Consider `--fix-dry-run`"
type: issue
state: closed
author: evanrittenhouse
labels: []
assignees: []
created_at: 2023-06-14T13:26:16Z
updated_at: 2023-06-14T15:17:11Z
url: https://github.com/astral-sh/ruff/issues/5081
synced_at: 2026-01-12T15:54:45Z
```

# Consider `--fix-dry-run`

---

_@evanrittenhouse_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

While implementing #4181, I saw [this comment](https://github.com/astral-sh/ruff/blob/main/crates/ruff_cli/src/lib.rs#L201) which mentions the implementation of `--fix-dry-run`. Is that something that we'd like to do? If so, how should the generated fixes be outputted? We could probably just use `TextEmitter` to write it to a file

P.S. Is adding labels permissions-restricted? I don't seem to be able to tag it

---

_Comment by @charliermarsh on 2023-06-14 14:58_

Hmm, I don't think this is relevant anymore, since we now _always_ generate fixes -- so there's no difference between `--fix-dry-run` and the default behavior.

---

_Comment by @charliermarsh on 2023-06-14 15:00_

(Gonna clean this up real quick...)

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-14 15:00_

---

_Closed by @charliermarsh on 2023-06-14 15:17_

---
