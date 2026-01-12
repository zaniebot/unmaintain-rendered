```yaml
number: 3209
title: Avoid boolean-trap rules for ConfigParser get() methods
type: pull_request
state: merged
author: monosans
labels:
  - bug
assignees: []
merged: true
base: main
head: patch-01
created_at: 2023-02-24T17:36:55Z
updated_at: 2023-02-24T17:52:42Z
url: https://github.com/astral-sh/ruff/pull/3209
synced_at: 2026-01-12T15:55:12Z
```

# Avoid boolean-trap rules for ConfigParser get() methods

---

_@monosans_

See <https://docs.python.org/3/library/configparser.html#fallback-values>.

Fallback value can be used both as a positional argument and as a keyword argument. But it seems to me that using it as a positional argument is more readable.

---

_Label `bug` added by @charliermarsh on 2023-02-24 17:52_

---

_Merged by @charliermarsh on 2023-02-24 17:52_

---

_Closed by @charliermarsh on 2023-02-24 17:52_

---

_Comment by @charliermarsh on 2023-02-24 17:52_

Thanks! Yeah this seems fine to me.

---
