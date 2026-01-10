```yaml
number: 4987
title: "Format `Constant::Ellipsis`"
type: issue
state: closed
author: evanrittenhouse
labels:
  - formatter
assignees: []
created_at: 2023-06-09T17:22:47Z
updated_at: 2023-06-13T21:34:23Z
url: https://github.com/astral-sh/ruff/issues/4987
synced_at: 2026-01-10T11:09:47Z
```

# Format `Constant::Ellipsis`

---

_Issue opened by @evanrittenhouse on 2023-06-09 17:22_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

From https://github.com/astral-sh/ruff/issues/4798


---

_Comment by @evanrittenhouse on 2023-06-09 17:23_

@MichaReiser can you please assign this to me? Interested in moving into the formatter

---

_Assigned to @evanrittenhouse by @MichaReiser on 2023-06-09 17:26_

---

_Comment by @MichaReiser on 2023-06-09 17:27_

Nice! 
I'll be off next week, but you can reach out to @konstin with questions. 

---

_Label `autoformatter` added by @charliermarsh on 2023-06-09 23:59_

---

_Comment by @evanrittenhouse on 2023-06-11 15:45_

@konstin Hey, just going through the code for this. I'm curious why [this line](https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_formatter/src/expression/expr_constant.rs#L22) doesn't already format `Constant::Ellipsis`? Happy to implement the issue, but not sure what else has to be done.

---

_Comment by @charliermarsh on 2023-06-11 16:08_

From my perspective, it does look complete, but I’ll let Konsti chime in.

---

_Comment by @konstin on 2023-06-12 07:44_

Sorry, that was already implemented, i updated #4798 now

---

_Comment by @evanrittenhouse on 2023-06-13 21:31_

All good, we can probably close this

---

_Closed by @charliermarsh on 2023-06-13 21:34_

---
