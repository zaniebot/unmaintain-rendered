---
number: 2357
title: "Add `bytes` to default value for `allow-magic-value-types`"
type: issue
state: closed
author: ngnpope
labels:
  - good first issue
  - configuration
assignees: []
created_at: 2023-01-30T17:20:41Z
updated_at: 2023-01-30T21:31:50Z
url: https://github.com/astral-sh/ruff/issues/2357
synced_at: 2026-01-10T01:22:40Z
---

# Add `bytes` to default value for `allow-magic-value-types`

---

_Issue opened by @ngnpope on 2023-01-30 17:20_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->


Currently (v0.0.237) [`allow-magic-value-types`](https://github.com/charliermarsh/ruff#allow-magic-value-types) has a value of `["str"]`. It would be good if this also ignored `bytes` by default.

---

_Label `good first issue` added by @charliermarsh on 2023-01-30 17:21_

---

_Label `configuration` added by @charliermarsh on 2023-01-30 17:21_

---

_Comment by @charliermarsh on 2023-01-30 17:21_

Makes sense!

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-30 21:31_

---

_Referenced in [astral-sh/ruff#2365](../../astral-sh/ruff/pulls/2365.md) on 2023-01-30 21:31_

---

_Closed by @charliermarsh on 2023-01-30 21:31_

---
