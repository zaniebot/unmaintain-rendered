```yaml
number: 4094
title: C419 autofix can remove comment
type: issue
state: closed
author: Skylion007
labels:
  - bug
assignees: []
created_at: 2023-04-25T14:52:39Z
updated_at: 2023-05-09T06:43:06Z
url: https://github.com/astral-sh/ruff/issues/4094
synced_at: 2026-01-10T11:09:47Z
```

# C419 autofix can remove comment

---

_Issue opened by @Skylion007 on 2023-04-25 14:52_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
We did a large change of the PyTorch codebase with ruff in https://github.com/pytorch/pytorch/pull/99890 by enabling C419 with autofixes. It mostly worked out of the box, but we did have to add back in one type comment for mypy that got deleted by the autofix: https://github.com/pytorch/pytorch/pull/99890/commits/ad98f63af41f54ebb76a817037d1fda0612e3e1a

---

_Label `bug` added by @charliermarsh on 2023-04-25 15:03_

---

_Comment by @charliermarsh on 2023-04-25 15:12_

👍 Thanks. We preserve some comments in the comprehension fixes, but clearly not that one :)

---

_Comment by @dhruvmanila on 2023-04-25 18:20_

I can pick this up, wanted to get into CST :)

---

_Comment by @dhruvmanila on 2023-04-25 18:45_

Ok, so it seems like we're not copying over the `lbracket` and `rbracket` fields from the `ListComp` node. Now, this is a challenge as we need to convert it into a `GeneratorExp` which only has the `lpar` and `rpar` fields. We'll need to pick the required parts from the Bracket nodes into the Parenthesis nodes.

---

_Assigned to @dhruvmanila by @MichaReiser on 2023-04-26 00:13_

---

_Closed by @MichaReiser on 2023-05-09 06:43_

---
