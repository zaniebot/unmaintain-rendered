```yaml
number: 1465
title: Make banned-api config setting optional
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: make-banned-api-config-optional
created_at: 2022-12-30T06:42:24Z
updated_at: 2022-12-30T12:09:57Z
url: https://github.com/astral-sh/ruff/pull/1465
synced_at: 2026-01-12T15:55:06Z
```

# Make banned-api config setting optional

---

_@not-my-profile_

In 9d34da23bde9c0788c654627373940ebf6f626f2
I introduced the [tool.ruff.flake8-tidy-imports.banned-api] config section but accidentally made it required, resulting in the following error if your pyproject.toml didn't specify the new section:

    error missing field `banned-api` for key `tool.ruff.flake8-tidy-imports`

This commit remedies that oversight.

Fixes #1464.

---

_Merged by @charliermarsh on 2022-12-30 12:09_

---

_Closed by @charliermarsh on 2022-12-30 12:09_

---
