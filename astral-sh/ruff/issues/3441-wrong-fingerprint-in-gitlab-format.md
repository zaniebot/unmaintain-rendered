```yaml
number: 3441
title: Wrong fingerprint in Gitlab format
type: issue
state: closed
author: Agalin
labels:
  - bug
assignees: []
created_at: 2023-03-10T14:32:20Z
updated_at: 2023-03-12T05:22:40Z
url: https://github.com/astral-sh/ruff/issues/3441
synced_at: 2026-01-10T11:09:46Z
```

# Wrong fingerprint in Gitlab format

---

_Issue opened by @Agalin on 2023-03-10 14:32_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Ruff version: 0.0.253

Gitlab Code Quality formatter [sets `fingerprint`](https://github.com/charliermarsh/ruff/blob/2383228709f109343de7821affd128ea00046677/crates/ruff_cli/src/printer.rs#L353) to a code of the issue, e.g. F401.

But this field should be a [unique value](https://docs.gitlab.com/ee/ci/testing/code_quality.html#implement-a-custom-tool) identifying a single violation, not its type. As a consequence, multiple violations of the same type are not visible (only the last one is shown).

It makes the feature unusable, for a project with currently 120 violations Gitlab only shows 7 (as there are 7 unique types of violations).

---

_Comment by @Agalin on 2023-03-10 15:39_

Just checked and it's not possible to work around this using external tools - report doesn't provide column position which means that it's not possible to distinguish multiple issues with the same description in the same line.

---

_Comment by @charliermarsh on 2023-03-10 16:03_

Makes sense, thanks for filing!

---

_Label `bug` added by @charliermarsh on 2023-03-10 16:03_

---

_Closed by @charliermarsh on 2023-03-12 05:22_

---
