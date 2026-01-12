```yaml
number: 3238
title: "[flake8-pyi]: PYI011, PYI014"
type: pull_request
state: merged
author: sbdchd
labels:
  - rule
assignees: []
merged: true
base: main
head: steve-pyi011-pyi014
created_at: 2023-02-26T16:58:22Z
updated_at: 2023-02-26T22:19:08Z
url: https://github.com/astral-sh/ruff/pull/3238
synced_at: 2026-01-12T04:39:44Z
```

# [flake8-pyi]: PYI011, PYI014

---

_Pull request opened by @sbdchd on 2023-02-26 16:58_

Implement PYI011 and PYI014 with the latest changes:

https://github.com/PyCQA/flake8-pyi/pull/326
https://github.com/PyCQA/flake8-pyi/issues/316

rel: https://github.com/charliermarsh/ruff/issues/848
rel: https://github.com/PyCQA/flake8-pyi/blob/4212bec43dbc4020a59b90e2957c9488575e57ba/pyi.py#L718

---

_Label `rule` added by @charliermarsh on 2023-02-26 22:09_

---

_Merged by @charliermarsh on 2023-02-26 22:11_

---

_Closed by @charliermarsh on 2023-02-26 22:11_

---

_@charliermarsh reviewed on 2023-02-26 22:18_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/simple_defaults.rs`:178 on 2023-02-26 22:18_

I changed the iteration logic here to mirror the way we match arguments to defaults in `generator.rs` -- namely, we need to take into account keywords, positional-only, etc.

---

_@charliermarsh reviewed on 2023-02-26 22:19_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/simple_defaults.rs`:69 on 2023-02-26 22:19_

I changed these to look at the size of the actual int in source code, which I _think_ mirrors the intent of `flake8-pyi`.

---
