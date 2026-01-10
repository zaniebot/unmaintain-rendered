```yaml
number: 4112
title: "Request: `docformatter` autofixing"
type: issue
state: closed
author: jamesbraza
labels:
  - question
assignees: []
created_at: 2023-04-26T06:30:25Z
updated_at: 2023-06-02T02:12:21Z
url: https://github.com/astral-sh/ruff/issues/4112
synced_at: 2026-01-10T11:09:47Z
```

# Request: `docformatter` autofixing

---

_Issue opened by @jamesbraza on 2023-04-26 06:30_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

https://github.com/charliermarsh/ruff/issues/1335 talks about adding [`docformatter`](https://github.com/PyCQA/docformatter) support.  Then it seems to get distracted by `pydocstyle` discussions and closed.

As of `ruff==0.0.263`, the `README.md` makes no mention of `docformatter` rules.

The request is to:
- Implement `docformatter` with autofixes (starting point is `docformatter --in-place`)
- Document it in the `README.md` and [docs](https://beta.ruff.rs/docs/rules/)

---

_Closed by @charliermarsh on 2023-04-26 15:15_

---

_Reopened by @charliermarsh on 2023-04-26 15:41_

---

_Comment by @charliermarsh on 2023-06-02 02:12_

Our support for `docformatter` is such that we have autofix enabled for the `pydocstyle` rules -- so, like `autoflake`, it doesn't fit cleanly into our existing documentation structures. I think I'll settle for this issue itself as the answer to the question, though open to other ways to clarify it.

---

_Closed by @charliermarsh on 2023-06-02 02:12_

---

_Label `question` added by @charliermarsh on 2023-06-02 02:12_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-02 02:12_

---
