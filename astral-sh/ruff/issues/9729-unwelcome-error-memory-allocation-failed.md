---
number: 9729
title: "Unwelcome Error: memory allocation failed"
type: issue
state: closed
author: rumbarum
labels: []
assignees: []
created_at: 2024-01-31T06:50:41Z
updated_at: 2024-01-31T07:31:29Z
url: https://github.com/astral-sh/ruff/issues/9729
synced_at: 2026-01-10T01:22:49Z
---

# Unwelcome Error: memory allocation failed

---

_Issue opened by @rumbarum on 2024-01-31 06:50_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

ruff 0.1.13 installed by brew
MBP M1 mac 14.1.2

I am using ruff on Pycharm Filewatcher to trigger when file saved, it had been worked for 2 weeks. Now it return this error message
```
/opt/homebrew/bin/ruff format --config ~/ruff.toml /pycharm/file/path
memory allocation of 8751445353061442862 bytes failed

Process finished with exit code 134 (interrupted by signal 6:SIGABRT)
```

This is my ruff config from ~/ruff.toml,

```
line-length = 100
exclude = ["*/__init__.py"]

[lint]
# F=pyflakes
# I=Isort
select = ["F", "I"]

#F541 f-string without any placeholders
#F841 local variable '...' is assigned to but never used
#F403 from {name} import * used; unable to detect undefined names
#F401 '...' imported but unused --> Makefile (make ruff)에서만, 사용 강제합니다.
ignore = ["F541", "F841", "F401", "F403"]
fixable = ["ALL"]

[isort]
relative-imports-order = "closest-to-furthest"

[format]
quote-style = "double"
indent-style = "space"
line-ending = "auto"
```


---

_Comment by @MichaReiser on 2024-01-31 07:31_

We're sorry you're encountering this issue and the unwelcome error message. 

We've been trying to identify the root cause of it in https://github.com/astral-sh/ruff/issues/8147 but have since been unable to reproduce it. It would be extremely helpful to us if you could share your cache state, commit version, and link to your project if it is an open source project so that we can reproduce locally and fix the issue. 

I'll close this issue to track the work in https://github.com/astral-sh/ruff/issues/8147

---

_Closed by @MichaReiser on 2024-01-31 07:31_

---
