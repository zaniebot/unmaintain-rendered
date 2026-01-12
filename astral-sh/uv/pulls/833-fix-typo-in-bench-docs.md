```yaml
number: 833
title: Fix typo in bench docs
type: pull_request
state: merged
author: bojanserafimov
labels: []
assignees: []
merged: true
base: main
head: bojan/fix-bench-docs
created_at: 2024-01-08T15:46:58Z
updated_at: 2024-01-08T17:35:28Z
url: https://github.com/astral-sh/uv/pull/833
synced_at: 2026-01-12T16:04:13Z
```

# Fix typo in bench docs

---

_@bojanserafimov_

Add missing /bench in the requirements path

---

_Review requested from @charliermarsh by @bojanserafimov on 2024-01-08 15:46_

---

_@zanieb approved on 2024-01-08 15:55_

Thanks!

---

_@zanieb reviewed on 2024-01-08 15:55_

---

_Review comment by @zanieb on `scripts/bench/__main__.py`:14 on 2024-01-08 15:55_

Do we need the `source ...`?

---

_@bojanserafimov reviewed on 2024-01-08 16:10_

---

_Review comment by @bojanserafimov on `scripts/bench/__main__.py`:14 on 2024-01-08 16:10_

It's implied, and people who have used venvs would know to do it, but lots of python devs just use `poetry shell` so they wouldn't think to do this. They'd just get an irrelevant-looking error later

---

_@zanieb reviewed on 2024-01-08 16:52_

---

_Review comment by @zanieb on `scripts/bench/__main__.py`:14 on 2024-01-08 16:52_

👍 

---

_Merged by @bojanserafimov on 2024-01-08 17:35_

---

_Closed by @bojanserafimov on 2024-01-08 17:35_

---

_Branch deleted on 2024-01-08 17:35_

---
