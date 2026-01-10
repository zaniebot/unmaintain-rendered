```yaml
number: 2394
title: "new rule for `not foo not in bar`"
type: issue
state: closed
author: spaceone
labels:
  - rule
assignees: []
created_at: 2023-01-31T13:25:44Z
updated_at: 2023-03-02T03:25:28Z
url: https://github.com/astral-sh/ruff/issues/2394
synced_at: 2026-01-10T11:09:45Z
```

# new rule for `not foo not in bar`

---

_Issue opened by @spaceone on 2023-01-31 13:25_

I have hard to understand code like:
`not foo not in bar`
Neither ruff not flake8 does complain about it.

As far as I tested this is equal to `foo in bar` !?!
So a new rule could add a check for this.

The code was created by https://github.com/charliermarsh/ruff/issues/2219.

---

_Label `rule` added by @charliermarsh on 2023-01-31 14:13_

---

_Comment by @charliermarsh on 2023-03-02 03:25_

We fixed the SIM rule to avoid generating expressions of that form (#2219) so I'm comfortable closing this one out.

---

_Closed by @charliermarsh on 2023-03-02 03:25_

---
