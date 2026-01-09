---
number: 10973
title: "E203: Incompatible with formatter when closing `]` is on its own line"
type: issue
state: closed
author: antolu
labels:
  - bug
  - help wanted
  - preview
assignees: []
created_at: 2024-04-15T22:22:46Z
updated_at: 2024-06-14T13:29:09Z
url: https://github.com/astral-sh/ruff/issues/10973
synced_at: 2026-01-07T13:12:15-06:00
---

# E203: Incompatible with formatter when closing `]` is on its own line

---

_Issue opened by @antolu on 2024-04-15 22:22_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
* A description of your environment (e.g., operating system, Python version, etc.).
-->
**Ruff version**: 0.1.7
**OS**: MacOS / ArchLinux / RHEL
**Python**: 3.11.5

I put both `ruff` and `ruff format` in my pre-commit hooks as follows:

```
  - repo: https://github.com/charliermarsh/ruff-pre-commit
    rev: "v0.3.7"
    hooks:
      - id: ruff
        args: [--fix, --exit-non-zero-on-fix]
      - id: ruff-format
```

Ruff config
```

[tool.ruff]

exclude = [
    ".bzr",
    ".direnv",
    ".eggs",
    ".git",
    ".hg",
    ".mypy_cache",
    ".nox",
    ".pants.d",
    ".pytype",
    ".ruff_cache",
    ".svn",
    ".tox",
    ".venv",
    "__pypackages__",
    "_build",
    "buck-out",
    "build",
    "dist",
    "node_modules",
    "venv",
    ".venv",
    "transformertf/hysteresis"
]

line-length = 88
target-version = "py311"

preview = true
unsafe-fixes = true

[tool.ruff.lint]
# All rules can be found here: https://docs.astral.sh/ruff/rules/
select = [
    "F",   # pyflakes
    "E",   # error  (pycodestyle)
    "W",   # warning (pycodestyle)
    "I",   # isort
    "N",   # naming  (pep8)
    "UP",  # pyupgrade
    "PLC", # convention  (pylint)
    "PLE", # error  (pylint)
    "PLW", # warning  (pylint)
    # "D",  # docstring (pydocstyle),  # not enabled by default
    "PD",   # pandas
    "TRY",  # tryceratops
    "NPY",  # numpy
    "PERF", # perflint
    "RUF",  # ruff
    # v flake8 v
    "B",
    "C4",
    "FBT",
    "A",
    "EM",
    "ISC",
    "FA",
    "G",
    "PIE",
    "PYI",
    "PT",
    "Q",
    "RSE",
    "RET",
    "SLF",
    "SIM",
]
fixable = ["ALL"]
ignore = ["E501", "W505", "ISC001", "PD901", "PLW2901", "N812", "N806", "G004"]

[tool.ruff.format]
preview = true
```

When running the hooks the `check --fix` seems to do something that `format` reverts.
<img width="572" alt="image" src="https://github.com/astral-sh/ruff-pre-commit/assets/9421489/951a3ac4-3018-4bb9-acb0-4b328d9c5acd">


Seems to be `ruff format` wanting to add space before colon:
<img width="1228" alt="image" src="https://github.com/astral-sh/ruff-pre-commit/assets/9421489/16293f47-e9a7-4d86-9b9b-bf2975f952ac">
<img width="1228" alt="image" src="https://github.com/astral-sh/ruff-pre-commit/assets/9421489/0c6cc289-0275-48b1-9fe2-324e01401962">



---

_Closed by @antolu on 2024-04-15 22:23_

---

_Reopened by @antolu on 2024-04-15 22:24_

---

_Comment by @charliermarsh on 2024-04-15 22:44_

Does `ruff check --fix` then _remove_ the colon?

---

_Comment by @charliermarsh on 2024-04-15 23:01_

Can you include the full contents of the file needed to reproduce this?

---

_Comment by @MichaReiser on 2024-04-16 07:22_

Note: I haven't been able to reproduce this by pasting the visible code into the playground. https://play.ruff.rs/f218da06-df69-4979-bd30-aea8d1572447

---

_Comment by @antolu on 2024-04-16 09:12_

I was able to reproduce it by putting my code in the playground and putting in some of my settings (even without those, I believe): https://play.ruff.rs/d0579912-6c11-41df-b509-78a79b416881. See line 201

Great tool btw! I think the "share" button does not actually copy the link in Safari though...

---

_Comment by @MichaReiser on 2024-04-16 09:19_

Thank you. That was very helpful! 

Okay I think I have a minimal repro. The linter only warns about the whitespace if the closing bracket is on its own line.

https://play.ruff.rs/bc811d2d-2386-4859-a948-dd9682df55e0

---

_Label `bug` added by @MichaReiser on 2024-04-16 09:20_

---

_Label `preview` added by @MichaReiser on 2024-04-16 09:20_

---

_Renamed from "ruff check --fix --preview --unsafe-fixes conflicts with ruff format" to "E203: Incompatible with formatter when closing `]` is on its own line" by @MichaReiser on 2024-04-16 09:21_

---

_Label `help wanted` added by @MichaReiser on 2024-04-16 09:21_

---

_Referenced in [astral-sh/ruff#10999](../../astral-sh/ruff/pulls/10999.md) on 2024-04-17 13:38_

---

_Comment by @dhruvmanila on 2024-06-14 12:55_

@MichaReiser Is this resolved by #10999 ?

---

_Comment by @MichaReiser on 2024-06-14 13:08_

Yeah I think so. At least it no longer shows up in the playground.

---

_Closed by @MichaReiser on 2024-06-14 13:08_

---

_Comment by @dhruvmanila on 2024-06-14 13:17_

> Yeah I think so. At least it no longer shows up in the playground.

I don't think the linked PR is released yet unless you're running it locally 😅 

---

_Comment by @MichaReiser on 2024-06-14 13:29_

I'm just gonna pretend that I tested it locally... But yes, the error no longer shows locally (but still does on play.ruff)

---
