---
number: 12082
title: "`UP037` with torchtyping can be annoying"
type: issue
state: open
author: fsouza
labels: []
assignees: []
created_at: 2024-06-28T04:15:36Z
updated_at: 2024-07-02T19:58:19Z
url: https://github.com/astral-sh/ruff/issues/12082
synced_at: 2026-01-10T01:22:51Z
---

# `UP037` with torchtyping can be annoying

---

_Issue opened by @fsouza on 2024-06-28 04:15_

(I wasn't sure if this was the ~same as #10812 so I figured opening a new issue wouldn't hurt)

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

When using [torchtyping](https://github.com/patrick-kidger/torchtyping), it's common to write annotations like `TensorType["batch", "x_channels"]`. ruff doesn't like that because of the quoted `"batch"` and `"x_channels"` (and removes the quote with autofix, leading to invalid code).

For now, we're resorting to `# noqa`, but I wonder if this is something ruff can handle.

---

_Comment by @knyazer on 2024-07-02 19:58_

Does not answer your question, but torchtyping is currently in an almost deprecated state: prefer to use [jaxtyping](https://github.com/patrick-kidger/jaxtyping) (it supports typing of anything numpy-ish, but the name is slightly misleading).

I think at some point I was reading some stuff from Patrick (the author of torchtyping) about what he uses in his workflow, and his observation was that most static analysis tools work poorly with his typing libraries, so the main intent behind using the type annotations was to use them with [beartype](https://github.com/beartype/beartype), a runtime typechecker.

---
