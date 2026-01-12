```yaml
number: 2014
title: Grammar fixes
type: pull_request
state: merged
author: scop
labels: []
assignees: []
merged: true
base: main
head: grammar
created_at: 2023-01-20T06:39:23Z
updated_at: 2023-01-20T16:28:42Z
url: https://github.com/astral-sh/ruff/pull/2014
synced_at: 2026-01-12T04:52:00Z
```

# Grammar fixes

---

_Pull request opened by @scop on 2023-01-20 06:39_

_No description provided._

---

_Review comment by @akx on `resources/test/fixtures/flake8_commas/COM81.py`:386 on 2023-01-20 09:47_

Since this is a change to the test fixture, it would probably also need the equivalent snapshot change.

---

_@akx reviewed on 2023-01-20 09:47_

---

_Review comment by @charliermarsh on `resources/test/fixtures/flake8_commas/COM81.py`:386 on 2023-01-20 12:44_

Sometimes :) It depends on whether the changed code is part of a violation, and whether it impacts the line and column numbers of the violation. In this case, this code is _correct_ to omit a trailing comma, I think, so there's no impact.

---

_@charliermarsh reviewed on 2023-01-20 12:44_

---

_Merged by @charliermarsh on 2023-01-20 12:44_

---

_Closed by @charliermarsh on 2023-01-20 12:44_

---

_Branch deleted on 2023-01-20 16:28_

---
