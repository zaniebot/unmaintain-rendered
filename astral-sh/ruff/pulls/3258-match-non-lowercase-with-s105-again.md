```yaml
number: 3258
title: Match non-lowercase with S105 again
type: pull_request
state: merged
author: scop
labels: []
assignees: []
merged: true
base: main
head: fix/s105-non-lowercase
created_at: 2023-02-27T20:41:26Z
updated_at: 2023-02-28T07:25:03Z
url: https://github.com/astral-sh/ruff/pull/3258
synced_at: 2026-01-12T04:39:44Z
```

# Match non-lowercase with S105 again

---

_Pull request opened by @scop on 2023-02-27 20:41_

Regression in cd9fbeb56042e1fa537626fca3e5e09ad8e6f54d / 0.0.253.

While at it, remove a comment made stale by the above mentioned commit.

---

_Renamed from "Match non-lowercase with S105" to "Match non-lowercase with S105 again" by @scop on 2023-02-27 20:43_

---

_@scop reviewed on 2023-02-27 20:44_

---

_Review comment by @scop on `crates/ruff/src/rules/flake8_bandit/helpers.rs`:22 on 2023-02-27 20:44_

I suppose we could lowercase `string` here instead of adding `(?i)` in the regex; former would require keeping the regex and the code here in sync, therefore opted to do the latter.

---

_@charliermarsh reviewed on 2023-02-27 21:32_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bandit/helpers.rs`:22 on 2023-02-27 21:32_

Let me benchmark them...

---

_Merged by @charliermarsh on 2023-02-27 21:38_

---

_Closed by @charliermarsh on 2023-02-27 21:38_

---

_Comment by @charliermarsh on 2023-02-27 21:38_

Thx!

---

_Branch deleted on 2023-02-28 07:25_

---
