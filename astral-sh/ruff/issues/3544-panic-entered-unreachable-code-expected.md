```yaml
number: 3544
title: "[Panic] entered unreachable code: Expected ImportNames::Aliases'"
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2023-03-15T17:12:11Z
updated_at: 2023-03-17T00:54:31Z
url: https://github.com/astral-sh/ruff/issues/3544
synced_at: 2026-01-12T15:54:43Z
```

# [Panic] entered unreachable code: Expected ImportNames::Aliases'

---

_@qarmin_

Ruff 0.0.255

command
```
ruff . --fix
```

crash
```
thread 'main' panicked at 'internal error: entered unreachable code: Expected ImportNames::Aliases', crates/ruff/src/rules/pyupgrade/rules/rewrite_mock_import.rs:187:9
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
Aborted (core dumped)
```

[12364mock (15th copy)0.py.zip](https://github.com/charliermarsh/ruff/files/10982822/12364mock.15th.copy.0.py.zip)



---

_Comment by @charliermarsh on 2023-03-15 18:00_

Thank you, I appreciate these a lot!

---

_Label `bug` added by @charliermarsh on 2023-03-15 23:23_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-16 00:04_

---

_Closed by @charliermarsh on 2023-03-17 00:54_

---
