```yaml
number: 2816
title: Allow non-verbose raise when cause is present
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/from-none
created_at: 2023-02-12T16:42:46Z
updated_at: 2023-02-12T16:48:14Z
url: https://github.com/astral-sh/ruff/pull/2816
synced_at: 2026-01-12T04:52:01Z
```

# Allow non-verbose raise when cause is present

---

_Pull request opened by @charliermarsh on 2023-02-12 16:42_

The motivating issue here is of the following form:

```py
try:
    raise Exception("We want to hide this error message")
except Exception:
    try:
        raise Exception("We want to show this")
    except Exception as exc:
        raise exc from None
```

However, I think we should avoid this if _any_ cause is present, since causes require a named exception.

Closes #2814.

---

_Merged by @charliermarsh on 2023-02-12 16:48_

---

_Closed by @charliermarsh on 2023-02-12 16:48_

---

_Branch deleted on 2023-02-12 16:48_

---
