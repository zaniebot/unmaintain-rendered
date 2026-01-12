```yaml
number: 2497
title: Fix an error in scripts/add_rule.py
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: pre-commit/fix-ruff-errors
created_at: 2023-02-02T20:15:03Z
updated_at: 2023-02-13T06:34:17Z
url: https://github.com/astral-sh/ruff/pull/2497
synced_at: 2026-01-12T04:52:00Z
```

# Fix an error in scripts/add_rule.py

---

_Pull request opened by @JonathanPlasse on 2023-02-02 20:15_

Ignore `PLR0915` Too many statements in `add_rule.py`.
Add `RUF100` to unfixable to avoid removing on `Fix all` `# noqa: PLR0915` which is not recognized by old Ruff versions.

---

_@charliermarsh reviewed on 2023-02-02 20:43_

---

_Review comment by @charliermarsh on `scripts/pyproject.toml`:6 on 2023-02-02 20:43_

Let's turn off all of the `pylint` rules please.

---

_Merged by @charliermarsh on 2023-02-02 20:58_

---

_Closed by @charliermarsh on 2023-02-02 20:58_

---

_@not-my-profile reviewed on 2023-02-13 06:34_

---

_Review comment by @not-my-profile on `scripts/pyproject.toml`:6 on 2023-02-13 06:34_

I really don't think turning off all `pylint` rules is a good idea ... we should eat our own dogfood and that includes not blindly disabling a whole linter.

---
