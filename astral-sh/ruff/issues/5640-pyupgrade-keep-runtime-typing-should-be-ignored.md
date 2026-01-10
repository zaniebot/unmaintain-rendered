```yaml
number: 5640
title: "`pyupgrade.keep-runtime-typing` should be ignored for `.pyi` stub files"
type: issue
state: closed
author: monosans
labels:
  - rule
  - accepted
assignees: []
created_at: 2023-07-10T10:30:56Z
updated_at: 2023-07-10T14:51:40Z
url: https://github.com/astral-sh/ruff/issues/5640
synced_at: 2026-01-10T11:09:48Z
```

# `pyupgrade.keep-runtime-typing` should be ignored for `.pyi` stub files

---

_Issue opened by @monosans on 2023-07-10 10:30_

Should the `pyupgrade.keep-runtime-typing` setting really apply to `.pyi` stub files? I think not, because they don't have runtime, so they should always use the new type annotation syntax.

---

_Comment by @charliermarsh on 2023-07-10 13:30_

I think you're right -- we can probably ignore `keep-runtime-typing` in those files.

---

_Label `rule` added by @charliermarsh on 2023-07-10 13:30_

---

_Label `accepted` added by @charliermarsh on 2023-07-10 13:30_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-10 13:56_

---

_Closed by @charliermarsh on 2023-07-10 14:51_

---
