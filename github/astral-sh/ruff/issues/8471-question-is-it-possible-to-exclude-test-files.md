---
number: 8471
title: "Question: is it possible to exclude test files specifically for pydocstyle "
type: issue
state: closed
author: neutrinoceros
labels:
  - question
assignees: []
created_at: 2023-11-03T13:22:04Z
updated_at: 2023-11-03T15:37:32Z
url: https://github.com/astral-sh/ruff/issues/8471
synced_at: 2026-01-07T13:12:15-06:00
---

# Question: is it possible to exclude test files specifically for pydocstyle 

---

_Issue opened by @neutrinoceros on 2023-11-03 13:22_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

I'm considering enabling ruff.pydocstyle for an existing project that is supposed to follow NumPy's style but isn't systematically checked yet. The issue I'm having is that most errors I'm getting are about missing docstrings in test functions. I would like to ignore tests files entirely but only for this subset of rules, is there a way to do it ? If not, I'd suggest adding a `extend-exclude` option to `[tool.ruff.pydocstyle]`.

Thanks again for building this wonderful tool !

---

_Comment by @charliermarsh on 2023-11-03 13:46_

Yeah, I'd recommend [`per-file-ignores`](https://docs.astral.sh/ruff/settings/#per-file-ignores):

```toml
[tool.ruff.per-file-ignores]
# Ignore all directories named `tests`.
"tests/**" = ["D"]
# Ignore all files that end in `_test.py`.
"*_test.py" = ["D"]
```

You can use this same approach to ignore other errors in tests too.

---

_Label `question` added by @charliermarsh on 2023-11-03 13:46_

---

_Comment by @neutrinoceros on 2023-11-03 15:24_

Perfect ! Sorry I missed that in the documentation, and thank you for answering so promptly !

---

_Closed by @neutrinoceros on 2023-11-03 15:24_

---

_Comment by @charliermarsh on 2023-11-03 15:37_

No worries!

---
