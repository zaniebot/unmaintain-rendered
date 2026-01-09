---
number: 8833
title: "Do not flag `PLR0124` for floats"
type: issue
state: closed
author: Skylion007
labels:
  - documentation
assignees: []
created_at: 2023-11-24T19:43:35Z
updated_at: 2023-12-07T02:33:40Z
url: https://github.com/astral-sh/ruff/issues/8833
synced_at: 2026-01-07T13:12:15-06:00
---

# Do not flag `PLR0124` for floats

---

_Issue opened by @Skylion007 on 2023-11-24 19:43_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
`PLR0124` does usually indicate a bug, but one place where it's used in a lot of codebases is to check whether a float value is NaN as it will fail this test only if it's a NaN. As such, it would be good to not flag these comparisons if they are float or add a setting to ignore them for floats.

---

_Comment by @dhruvmanila on 2023-11-25 00:13_

Can you expand a bit as I'm unable to follow? Do you mean that if `foo` is `float('nan')` then `foo == foo` will give `False` when it's a NaN and that's what the code is intended to check?

---

_Comment by @Skylion007 on 2023-11-25 18:49_

Exactly @dhruvmanila.

---

_Comment by @Skylion007 on 2023-11-25 18:50_

Although I suppose `math.isnan` might be a more idiomatic way to check it.

---

_Comment by @zanieb on 2023-11-27 16:09_

Yeah I don't think using `foo == foo` to detect NaN is something we want to encourage — we could add a case to `PLR0124` that suggests `math.isnan` or a new rule for that?

---

_Comment by @charliermarsh on 2023-11-29 04:48_

I might settle for just a note in the documentation here as to how users should handle this specific case.

---

_Label `documentation` added by @dhruvmanila on 2023-11-30 22:12_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-07 02:18_

---

_Referenced in [astral-sh/ruff#9033](../../astral-sh/ruff/pulls/9033.md) on 2023-12-07 02:27_

---

_Closed by @charliermarsh on 2023-12-07 02:33_

---
