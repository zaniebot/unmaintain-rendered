```yaml
number: 2912
title: "[docs] `flake8-self` Private member access docs"
type: pull_request
state: merged
author: Sawbez
labels: []
assignees: []
merged: true
base: main
head: more-docs
created_at: 2023-02-15T05:11:21Z
updated_at: 2023-02-15T15:42:39Z
url: https://github.com/astral-sh/ruff/pull/2912
synced_at: 2026-01-12T15:55:12Z
```

# [docs] `flake8-self` Private member access docs

---

_@Sawbez_

This adds the documentation for `flake8-self`.

---

_@not-my-profile reviewed on 2023-02-15 06:13_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/flake8_self/rules/private_member_access.rs`:16 on 2023-02-15 06:13_

I think right here we should explain what "declared private" means.

---

_@not-my-profile reviewed on 2023-02-15 06:17_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/flake8_self/rules/private_member_access.rs`:24 on 2023-02-15 06:17_

What to do depends on a case by case basis. We should not recommend making the member public as the primary solution.

---

_Merged by @charliermarsh on 2023-02-15 15:42_

---

_Closed by @charliermarsh on 2023-02-15 15:42_

---
