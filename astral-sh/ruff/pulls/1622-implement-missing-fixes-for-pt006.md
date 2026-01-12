```yaml
number: 1622
title: "Implement missing fixes for `PT006`"
type: pull_request
state: merged
author: edgarrmondragon
labels: []
assignees: []
merged: true
base: main
head: pt006-missing-fixes
created_at: 2023-01-04T03:12:39Z
updated_at: 2023-01-04T03:53:41Z
url: https://github.com/astral-sh/ruff/pull/1622
synced_at: 2026-01-12T15:55:06Z
```

# Implement missing fixes for `PT006`

---

_@edgarrmondragon_

Closes https://github.com/charliermarsh/ruff/issues/1620


---

_@charliermarsh reviewed on 2023-01-04 03:15_

---

_Review comment by @charliermarsh on `src/flake8_pytest_style/plugins/parametrize.rs`:195 on 2023-01-04 03:15_

Do you mind adding these to the `fixable()` list in `registry.rs` (and the `commit()` list)?

---

_Review comment by @edgarrmondragon on `src/flake8_pytest_style/plugins/parametrize.rs`:195 on 2023-01-04 03:18_

`ParametrizeNamesWrongType` is already in both lists. Is there another place where it should be added?

---

_@edgarrmondragon reviewed on 2023-01-04 03:18_

---

_@charliermarsh reviewed on 2023-01-04 03:34_

---

_Review comment by @charliermarsh on `src/flake8_pytest_style/plugins/parametrize.rs`:195 on 2023-01-04 03:34_

Oh sorry, I was misled because the list didn't update -- but this is basically different cases of a rule that was already fixable _sometimes_, is that right?

---

_Review comment by @edgarrmondragon on `src/flake8_pytest_style/plugins/parametrize.rs`:195 on 2023-01-04 03:36_

@charliermarsh that's correct. Not all cases were being fixed.

---

_@edgarrmondragon reviewed on 2023-01-04 03:36_

---

_Merged by @charliermarsh on 2023-01-04 03:52_

---

_Closed by @charliermarsh on 2023-01-04 03:52_

---

_Branch deleted on 2023-01-04 03:53_

---
