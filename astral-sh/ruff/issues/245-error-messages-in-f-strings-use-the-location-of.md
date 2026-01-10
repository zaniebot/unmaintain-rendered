```yaml
number: 245
title: Error messages in f-strings use the location of the f-string
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2022-09-21T17:43:52Z
updated_at: 2022-12-31T04:44:18Z
url: https://github.com/astral-sh/ruff/issues/245
synced_at: 2026-01-10T12:05:20Z
```

# Error messages in f-strings use the location of the f-string

---

_Issue opened by @charliermarsh on 2022-09-21 17:43_

Similar to #145 but less severe. We now use the location of the f-string when reporting any linter errors within an f-string, which is inaccurate (but an improvement).


---

_Label `enhancement` added by @charliermarsh on 2022-09-21 17:43_

---

_Comment by @charliermarsh on 2022-12-31 04:44_

Fixed by @harupy in https://github.com/charliermarsh/ruff/pull/1494.

---

_Closed by @charliermarsh on 2022-12-31 04:44_

---
