```yaml
number: 616
title: Fix B015 false positive on comparison deep inside expression statement
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: pointy-comparison
created_at: 2022-11-06T00:03:26Z
updated_at: 2022-11-06T01:35:42Z
url: https://github.com/astral-sh/ruff/pull/616
synced_at: 2026-01-12T05:48:45Z
```

# Fix B015 false positive on comparison deep inside expression statement

---

_Pull request opened by @andersk on 2022-11-06 00:03_

We only want to complain about a “pointless comparison” if the comparison is the entire statement, not if it’s just part of a statement.

```python
x < y  # error
print(x < y)  # no error
```

---

_Merged by @charliermarsh on 2022-11-06 00:13_

---

_Closed by @charliermarsh on 2022-11-06 00:13_

---

_Branch deleted on 2022-11-06 00:32_

---

_Comment by @harupy on 2022-11-06 01:35_

thanks for the fix!

---
