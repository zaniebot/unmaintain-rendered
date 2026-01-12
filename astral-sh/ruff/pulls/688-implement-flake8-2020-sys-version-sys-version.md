```yaml
number: 688
title: Implement flake8-2020 (sys.version, sys.version_info misuse)
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: flake8-2020
created_at: 2022-11-12T00:12:36Z
updated_at: 2022-11-16T06:04:58Z
url: https://github.com/astral-sh/ruff/pull/688
synced_at: 2026-01-12T05:48:45Z
```

# Implement flake8-2020 (sys.version, sys.version_info misuse)

---

_Pull request opened by @andersk on 2022-11-12 00:12_

https://pypi.org/project/flake8-2020/
https://github.com/asottile/flake8-2020

---

_Comment by @charliermarsh on 2022-11-12 00:15_

Epic PR

---

_@charliermarsh reviewed on 2022-11-12 00:19_

---

_Review comment by @charliermarsh on `src/flake8_2020/plugins.rs`:172 on 2022-11-12 00:19_

Curious if you had any thoughts on https://github.com/charliermarsh/ruff/pull/628? It'd relax the requirement that we always validate check enablement, but we'd end up doing some unnecessary work at times.

---

_@charliermarsh reviewed on 2022-11-12 00:20_

---

_Review comment by @charliermarsh on `src/check_ast.rs`:40 on 2022-11-12 00:20_

I don't love that this list lives here, and yet a bunch of modules implicitly rely on entries being included here. Maybe we should just track everything, it's not like it's super expensive. (This isn't feedback on the PR, more thinking aloud.)

---

_Comment by @charliermarsh on 2022-11-12 00:20_

Looks great, will merge tonight.

---

_Merged by @charliermarsh on 2022-11-12 01:39_

---

_Closed by @charliermarsh on 2022-11-12 01:39_

---

_Branch deleted on 2022-11-16 06:04_

---
