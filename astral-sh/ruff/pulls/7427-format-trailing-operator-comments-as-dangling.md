```yaml
number: 7427
title: Format trailing operator comments as dangling
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/dangle
created_at: 2023-09-15T23:29:41Z
updated_at: 2023-09-16T00:34:10Z
url: https://github.com/astral-sh/ruff/pull/7427
synced_at: 2026-01-12T15:55:23Z
```

# Format trailing operator comments as dangling

---

_@charliermarsh_

## Summary

Given a trailing operator comment in a unary expression, like:

```python
if (
  not  # comment
  a):
    ...
```

We were attaching these to the operand (`a`), but formatting them in the unary operator via special handling. Parents shouldn't format the comments of their children, so this instead attaches them as dangling comments on the unary expression. (No intended change in formatting.)

---

_Label `formatter` added by @charliermarsh on 2023-09-15 23:29_

---

_Merged by @charliermarsh on 2023-09-16 00:34_

---

_Closed by @charliermarsh on 2023-09-16 00:34_

---

_Branch deleted on 2023-09-16 00:34_

---
