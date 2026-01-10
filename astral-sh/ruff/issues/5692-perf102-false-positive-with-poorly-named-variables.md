```yaml
number: 5692
title: PERF102 - False Positive with Poorly Named Variables
type: issue
state: closed
author: Skylion007
labels:
  - bug
assignees: []
created_at: 2023-07-11T16:41:43Z
updated_at: 2023-07-14T19:42:49Z
url: https://github.com/astral-sh/ruff/issues/5692
synced_at: 2026-01-10T11:09:48Z
```

# PERF102 - False Positive with Poorly Named Variables

---

_Issue opened by @Skylion007 on 2023-07-11 16:41_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
```
-                    for (i, j), _ in smat.items():
+                    for (i, j) in smat.keys():
                         diag_entries[(i + rmax, j + cmax)] = _
```
This code snippit is from sympy/matrices/common.py, this uses the latest stable ruff version and just has PERF102 manually selected using the command `ruff --select PERF --fix .`

I know that the `_` prefix is often used to look for variables that should be unused in a loop, but ruff should check if the variable is actually unused before autofixing it to prevent bugprone autofixes. It should at least handle this trivial case.



---

_Renamed from "PERF102 - False Positive + Bad Fix" to "PERF102 - False Positive with Poorly Named Variables" by @Skylion007 on 2023-07-11 16:42_

---

_Comment by @charliermarsh on 2023-07-11 16:45_

Yeah we can support this.

---

_Label `bug` added by @charliermarsh on 2023-07-11 16:45_

---

_Comment by @zanieb on 2023-07-11 17:18_

Wow that's madness :) We should add a rule that checks for usage of `_` outside of unpacking!

---

_Comment by @dhruvmanila on 2023-07-11 17:37_

Yeah, I've seen this kind of usage in the wild especially in comprehensions for one off variables.

---

_Comment by @charliermarsh on 2023-07-13 00:19_

`crates/ruff/src/rules/flake8_bugbear/rules/unused_loop_control_variable.rs` is a similar rule (it detects unused loop control variables more generally) that we can use as a reference.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-14 13:46_

---

_Closed by @charliermarsh on 2023-07-14 19:42_

---
