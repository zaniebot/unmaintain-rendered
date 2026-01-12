```yaml
number: 3149
title: "[flake8-pie] Unnecessary list comprehension, with autofix (PIE802)"
type: pull_request
state: merged
author: matthewlloyd
labels: []
assignees: []
merged: true
base: main
head: pie802
created_at: 2023-02-22T23:43:45Z
updated_at: 2023-02-23T02:15:23Z
url: https://github.com/astral-sh/ruff/pull/3149
synced_at: 2026-01-12T15:55:12Z
```

# [flake8-pie] Unnecessary list comprehension, with autofix (PIE802)

---

_@matthewlloyd_

Closes https://github.com/charliermarsh/ruff/issues/3144

---

_@charliermarsh reviewed on 2023-02-23 01:00_

---

_Review comment by @charliermarsh on `crates/ruff/src/registry.rs`:494 on 2023-02-23 01:00_

Our convention is that rules should be named such that "allow X" would be a sensical phrase. I guess the flake8-pie rules aren't quite adherent to that. But we should try to use it when adding new rules. So maybe, like, `unnecessary-comprehension-any-all`?

---

_@charliermarsh reviewed on 2023-02-23 01:01_

---

_Review comment by @charliermarsh on `crates/ruff/src/registry.rs`:494 on 2023-02-23 01:01_

(Otherwise, all looks good to me.)

---

_@matthewlloyd reviewed on 2023-02-23 01:42_

---

_Review comment by @matthewlloyd on `crates/ruff/src/registry.rs`:494 on 2023-02-23 01:42_

Sounds good, done!

---

_Merged by @charliermarsh on 2023-02-23 01:58_

---

_Closed by @charliermarsh on 2023-02-23 01:58_

---

_Branch deleted on 2023-02-23 02:15_

---
