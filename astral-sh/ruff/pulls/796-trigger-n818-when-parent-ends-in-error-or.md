```yaml
number: 796
title: Trigger N818 when parent ends in Error or Exception
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/error
created_at: 2022-11-17T19:51:09Z
updated_at: 2022-11-17T20:04:32Z
url: https://github.com/astral-sh/ruff/pull/796
synced_at: 2026-01-12T05:48:45Z
```

# Trigger N818 when parent ends in Error or Exception

---

_Pull request opened by @charliermarsh on 2022-11-17 19:51_

Resolves #783.

---

_Merged by @charliermarsh on 2022-11-17 19:51_

---

_Closed by @charliermarsh on 2022-11-17 19:51_

---

_Branch deleted on 2022-11-17 19:51_

---

_@andersk reviewed on 2022-11-17 19:55_

---

_Review comment by @andersk on `src/pep8_naming/checks.rs`:227 on 2022-11-17 19:55_

Do we want to exempt `BaseException`, maybe with `id == "Exception" || id.ends_with("Error")`?

---

_@charliermarsh reviewed on 2022-11-17 20:04_

---

_Review comment by @charliermarsh on `src/pep8_naming/checks.rs`:227 on 2022-11-17 20:04_

https://github.com/charliermarsh/ruff/pull/798

---
