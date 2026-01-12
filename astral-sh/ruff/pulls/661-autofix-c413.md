```yaml
number: 661
title: Autofix C413
type: pull_request
state: merged
author: squiddy
labels: []
assignees: []
merged: true
base: main
head: autofix-c413
created_at: 2022-11-08T20:37:34Z
updated_at: 2022-12-27T05:42:52Z
url: https://github.com/astral-sh/ruff/pull/661
synced_at: 2026-01-12T05:36:31Z
```

# Autofix C413

---

_Pull request opened by @squiddy on 2022-11-08 20:37_

This also adds a new dev command to print the ast as seen by LibCST.

---

_@charliermarsh reviewed on 2022-11-08 21:12_

---

_Review comment by @charliermarsh on `ruff_dev/src/main.rs`:27 on 2022-11-08 21:12_

I renamed this to `PrintCST`, since the CST in `LibCST` stands for concrete syntax tree (as opposed to abstract syntax tree).


---

_Merged by @charliermarsh on 2022-11-08 21:12_

---

_Closed by @charliermarsh on 2022-11-08 21:12_

---

_@squiddy reviewed on 2022-11-09 04:21_

---

_Review comment by @squiddy on `ruff_dev/src/main.rs`:27 on 2022-11-09 04:21_

Didn't think this one through. Thanks.

---

_Branch deleted on 2022-12-27 05:42_

---
