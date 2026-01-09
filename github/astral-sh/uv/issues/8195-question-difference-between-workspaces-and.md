---
number: 8195
title: "Question:  Difference between workspaces and editable-dependencies"
type: issue
state: closed
author: xiaoxiangmoe
labels:
  - question
assignees: []
created_at: 2024-10-15T03:39:47Z
updated_at: 2024-10-16T19:35:42Z
url: https://github.com/astral-sh/uv/issues/8195
synced_at: 2026-01-07T13:12:17-06:00
---

# Question:  Difference between workspaces and editable-dependencies

---

_Issue opened by @xiaoxiangmoe on 2024-10-15 03:39_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

What's the difference between [workspaces](https://docs.astral.sh/uv/concepts/workspaces) and [editable-dependencies](https://docs.astral.sh/uv/concepts/dependencies/#editable-dependencies)? It seems that they all can make we add dir `packages/bird-feeder` as package `bird-feeder`.

Should all workspaces be editable-dependencies?



---

_Comment by @konstin on 2024-10-15 08:11_

Editables are an older, standardized mechanism. They are a special mode to use with path dependencies. You could use them in `requirements.txt` to emulate workspaces, e.g.:

```
-e .
-e packages/bird-feeder
```

Editables don't say anything about the relationship these packages have to each other, they just a path dependency that says that a certain package should be built and installed in editable mode instead of in regular mode.

Workspaces are a uv feature that allows managing multiple packages in a single repository together. It does more than editables (or path dependencies in general), such as sharing a lockfile and sources. By default, all members of the workspace are installed as editables.

---

_Label `question` added by @konstin on 2024-10-15 08:11_

---

_Comment by @charliermarsh on 2024-10-16 19:34_

A workspace member is a package within a workspace. A package can be installed as editable or non-editable. By default, we install all workspace members in "editable mode", but you can pass `--no-editable` to disable that.

---

_Closed by @charliermarsh on 2024-10-16 19:34_

---
