```yaml
number: 634
title: remove unnecessary __future__ imports
type: pull_request
state: merged
author: chammika-become
labels: []
assignees: []
merged: true
base: main
head: rm-future-imports
created_at: 2022-11-07T03:45:17Z
updated_at: 2022-11-08T03:26:57Z
url: https://github.com/astral-sh/ruff/pull/634
synced_at: 2026-01-12T05:48:45Z
```

# remove unnecessary __future__ imports

---

_Pull request opened by @chammika-become on 2022-11-07 03:45_

This PR addresses Issue #312
- Only perform checks
- Auto fix to be attempted in a later PR

---

_Comment by @charliermarsh on 2022-11-07 03:47_

Dang this is great! Thanks for the PR! Will review tonight or tomorrow morning.

---

_Review comment by @charliermarsh on `src/pyupgrade/checks.rs`:9 on 2022-11-07 14:27_

Nit: Should this be `PY33_PLUS_REMOVE_FUTURES`? Or is it intentionally `PY3_PLUS`?

---

_@charliermarsh reviewed on 2022-11-07 14:27_

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/unnecessary_future_import.rs`:15 on 2022-11-07 14:27_

Mind removing these three lines for now?

---

_@charliermarsh reviewed on 2022-11-07 14:27_

---

_Comment by @charliermarsh on 2022-11-07 14:27_

Looks great! Happy to merge once minor comments are addressed.

---

_Review comment by @chammika-become on `src/pyupgrade/checks.rs`:9 on 2022-11-08 03:16_

Thanks for the catch It should be `PY33_`. 
I was modeling the remove feature after `pyupgrade` which support version 3+, 3,7, 3,10 etc. Then I realized `ruff` starts with `Py33`. 

---

_@chammika-become reviewed on 2022-11-08 03:16_

---

_Marked ready for review by @charliermarsh on 2022-11-08 03:17_

---

_Merged by @charliermarsh on 2022-11-08 03:26_

---

_Closed by @charliermarsh on 2022-11-08 03:26_

---
