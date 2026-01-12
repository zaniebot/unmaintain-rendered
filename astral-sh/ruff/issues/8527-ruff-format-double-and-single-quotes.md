```yaml
number: 8527
title: ruff format double and single quotes
type: issue
state: closed
author: Xiwang-Li-Walmart
labels:
  - question
assignees: []
created_at: 2023-11-06T20:46:58Z
updated_at: 2023-11-06T20:50:30Z
url: https://github.com/astral-sh/ruff/issues/8527
synced_at: 2026-01-12T15:54:48Z
```

# ruff format double and single quotes

---

_@Xiwang-Li-Walmart_

Can we have an option to select either double or single quote for ruff formatting? 
and even just ignore  this double or single quote formatting by add `-S` like in Black?

Thank you.


<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Comment by @charliermarsh on 2023-11-06 20:49_

I believe this exists as [`quote-style`](https://docs.astral.sh/ruff/settings/#format-quote-style), e.g., in your `pyproject.toml`:

```toml
[tool.ruff.format]
quote-style = "single"
```

---

_Label `question` added by @charliermarsh on 2023-11-06 20:49_

---

_Comment by @zanieb on 2023-11-06 20:49_

See also https://github.com/astral-sh/ruff/discussions/7305

---

_Closed by @zanieb on 2023-11-06 20:49_

---
