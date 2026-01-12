```yaml
number: 14353
title: Misleading documentation for uncapitalized-environment-variables (SIM112)
type: issue
state: open
author: Zeckie
labels:
  - documentation
assignees: []
created_at: 2024-11-15T07:24:16Z
updated_at: 2024-11-15T07:49:44Z
url: https://github.com/astral-sh/ruff/issues/14353
synced_at: 2026-01-12T15:54:53Z
```

# Misleading documentation for uncapitalized-environment-variables (SIM112)

---

_@Zeckie_

https://docs.astral.sh/ruff/rules/uncapitalized-environment-variables/

> On Windows, environment variables are case-insensitive and are converted to uppercase, so using lowercase environment variables can lead to subtle bugs.

The current wording is a little vague, and makes it sound like it is Windows that converts the environment variable names to uppercase. In reality, Windows  preserves the case of environment variables. Some environment variables that are built into windows use mixed / lower case (for example `Path`, `SystemRoot`, `windir`). The limitation / case conversion seems to be in `os.environ` (more in https://github.com/python/cpython/issues/73010). 

Also, it doesn't explain or give any examples of how using lowercase variable names might lead to the "subtle bugs".

---

_Label `documentation` added by @MichaReiser on 2024-11-15 07:49_

---
