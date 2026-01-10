```yaml
number: 2024
title: "Implement `flake8-executable`"
type: issue
state: closed
author: sbrugman
labels:
  - plugin
assignees: []
created_at: 2023-01-20T11:40:35Z
updated_at: 2023-01-24T21:37:35Z
url: https://github.com/astral-sh/ruff/issues/2024
synced_at: 2026-01-10T11:09:44Z
```

# Implement `flake8-executable`

---

_Issue opened by @sbrugman on 2023-01-20 11:40_

- [x] [`EXE001`](https://github.com/xuhdev/flake8-executable): Shebang is present but the file is not executable.
- [x] [`EXE002`](https://github.com/xuhdev/flake8-executable): The file is executable but no shebang is present.
- [x] [`EXE003`](https://github.com/xuhdev/flake8-executable): Shebang is present but does not contain "python".
- [x] [`EXE004`](https://github.com/xuhdev/flake8-executable): There is whitespace before shebang.
- [x] [`EXE005`](https://github.com/xuhdev/flake8-executable): There are blank or comment lines before shebang.

Tracking issue for `flake8-executable`:
> Very often, developers mess up the executable permissions and shebangs of Python files. For example,
 sometimes the executable permission was accidentally granted, sometimes it is forgotten. Moreover,
 this should be consistent with Top-level code environment checks.

(Plugin would already be relevant for `scripts/`)


---

_Label `plugin` added by @charliermarsh on 2023-01-20 12:41_

---

_Comment by @edgarrmondragon on 2023-01-24 21:36_

This can probably be closed now if `EXE006` and `EXE007` from https://github.com/xuhdev/flake8-executable/pull/66 won't be implemented for the time being.

---

_Comment by @charliermarsh on 2023-01-24 21:37_

Good call.

---

_Closed by @charliermarsh on 2023-01-24 21:37_

---
