```yaml
number: 1497
title: Remove unused snapshots
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: remove-unused-snapshots
created_at: 2022-12-31T05:16:09Z
updated_at: 2023-01-01T04:57:07Z
url: https://github.com/astral-sh/ruff/pull/1497
synced_at: 2026-01-12T05:36:31Z
```

# Remove unused snapshots

---

_Pull request opened by @harupy on 2022-12-31 05:16_

How I found unused snapshots:

```
cargo insta test --delete-unreferenced-snapshots
```

It'd be nice if we could detect them in CI.

---

_Comment by @harupy on 2022-12-31 05:25_

~Note this PR doesn't remove `src/pyflakes/snapshots/ruff__pyflakes__tests__F831_F831.py.snap` since https://github.com/charliermarsh/ruff/pull/1496 does.~

---

_Review comment by @harupy on `.github/workflows/ci.yaml`:128 on 2022-12-31 11:42_

Verified this check works as expected:

#### When unused snapshots exist:
https://github.com/charliermarsh/ruff/actions/runs/3811933348/jobs/6484869555 -> failed

#### When unused snapshots don't exist:
https://github.com/charliermarsh/ruff/actions/runs/3811979784/jobs/6484938134 -> success

---

_@harupy reviewed on 2022-12-31 11:42_

---

_Comment by @charliermarsh on 2022-12-31 12:40_

Thank you :)

---

_@charliermarsh reviewed on 2022-12-31 12:40_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yaml`:128 on 2022-12-31 12:40_

The other option would be to run `cargo insta test --delete-unreferenced-snapshots` then `git diff`... but that requires running tests twice.

---

_Merged by @charliermarsh on 2022-12-31 12:40_

---

_Closed by @charliermarsh on 2022-12-31 12:40_

---

_@max-sixty reviewed on 2022-12-31 19:21_

---

_Review comment by @max-sixty on `.github/workflows/ci.yaml`:128 on 2022-12-31 19:21_

Forgive the drive-by but I came across this — another approach is running `cargo insta test --delete-unreferenced-snapshots` as the main test command, and then doing `git diff` — then tests only need to run once. And `cargo insta test` covers all tests.

---

_@harupy reviewed on 2023-01-01 04:57_

---

_Review comment by @harupy on `.github/workflows/ci.yaml`:128 on 2023-01-01 04:57_

@max-sixty Thanks for the comment. I thought `cargo insta test` only runs snapshot tests but looks like I was wrong. I'll file a RP :)

---
