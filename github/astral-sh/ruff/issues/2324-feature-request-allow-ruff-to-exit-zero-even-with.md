---
number: 2324
title: "feature request: Allow ruff to exit zero even with found violations"
type: issue
state: closed
author: 93578237
labels: []
assignees: []
created_at: 2023-01-29T12:17:44Z
updated_at: 2023-01-29T13:14:16Z
url: https://github.com/astral-sh/ruff/issues/2324
synced_at: 2026-01-07T13:12:14-06:00
---

# feature request: Allow ruff to exit zero even with found violations

---

_Issue opened by @93578237 on 2023-01-29 12:17_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->
Hi, thank you for ruff. I want to run ruff using an external tool (like pytorch's `lintrunner`) which requires linters to always exit 0. Would you mind adding a new flag to allow this behaviour?

---

_Comment by @not-my-profile on 2023-01-29 13:11_

This already exists:

```
  -e, --exit-zero
          Exit with status code "0", even upon detecting lint violations
```

---

_Comment by @93578237 on 2023-01-29 13:13_

@not-my-profile thanks, ~~but its not in `ruff -h`~~ found in `ruff check -h`

---

_Closed by @93578237 on 2023-01-29 13:14_

---

_Comment by @not-my-profile on 2023-01-29 13:14_

Yes because the linting functionality has moved to the `check` subcommand, see `ruff help check` or `ruff check -h` or `ruff check --help`.

---
