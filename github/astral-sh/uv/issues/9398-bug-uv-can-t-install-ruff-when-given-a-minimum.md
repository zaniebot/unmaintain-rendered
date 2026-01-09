---
number: 9398
title: "Bug: Uv can't install Ruff when given a minimum bound spec"
type: issue
state: closed
author: yehoshuadimarsky
labels: []
assignees: []
created_at: 2024-11-24T16:31:23Z
updated_at: 2024-11-24T16:55:13Z
url: https://github.com/astral-sh/uv/issues/9398
synced_at: 2026-01-07T13:12:18-06:00
---

# Bug: Uv can't install Ruff when given a minimum bound spec

---

_Issue opened by @yehoshuadimarsky on 2024-11-24 16:31_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Setup:

```
uv init --name bug-report-1 --lib --package ./bug-report-1
cd bug-report-1
uv add 'ruff>=6'
```

Error:
```
Using CPython 3.13.0
Creating virtual environment at: .venv
  × No solution found when resolving dependencies:
  ╰─▶ Because only ruff<=0.8.0 is available and your project depends on ruff>=6, we can conclude that your project's requirements are
      unsatisfiable.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
```

My env:
MacOS latest
```
uv --version
uv 0.5.4 (Homebrew 2024-11-20)
```

---

_Comment by @charliermarsh on 2024-11-24 16:36_

Do you mean `ruff>=0.6`?

---

_Comment by @yehoshuadimarsky on 2024-11-24 16:37_

OMG i'm so sorry, yes such a dumb mistake, nice catch.

---

_Closed by @yehoshuadimarsky on 2024-11-24 16:37_

---

_Comment by @charliermarsh on 2024-11-24 16:55_

Haha it’s all good! Glad it’s an easy fix.

---
