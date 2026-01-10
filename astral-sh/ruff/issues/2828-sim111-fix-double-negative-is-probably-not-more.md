```yaml
number: 2828
title: "SIM111 fix: Double negative is probably not more understandable code"
type: issue
state: closed
author: cclauss
labels:
  - fixes
assignees: []
created_at: 2023-02-12T22:24:04Z
updated_at: 2023-02-12T22:39:31Z
url: https://github.com/astral-sh/ruff/issues/2828
synced_at: 2026-01-10T11:09:45Z
```

# SIM111 fix: Double negative is probably not more understandable code

---

_Issue opened by @cclauss on 2023-02-12 22:24_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
% `ruff check --select=SIM111 --fix .`
```
for field in self:
    if field.value() not in field.field.empty_values:
        return False
return True
# -->
return all(not field.value() not in field.field.empty_values for field in self)  # Double negative!
```

---

_Comment by @charliermarsh on 2023-02-12 22:25_

Heh this has actually been raised a few times. We should probably special-case it by removing both `not`s?

---

_Label `autofix` added by @charliermarsh on 2023-02-12 22:26_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-12 22:26_

---

_Closed by @charliermarsh on 2023-02-12 22:39_

---
