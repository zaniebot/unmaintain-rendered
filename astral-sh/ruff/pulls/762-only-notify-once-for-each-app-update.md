```yaml
number: 762
title: Only notify once for each app update
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/once
created_at: 2022-11-15T21:14:06Z
updated_at: 2022-11-15T21:14:11Z
url: https://github.com/astral-sh/ruff/pull/762
synced_at: 2026-01-12T15:55:05Z
```

# Only notify once for each app update

---

_@charliermarsh_

I thought `update-informer` handled this, but apparently not! Instead, the `update-informer` setting controls how frequently we _poll_ for a new version. Now, we only print the reminder once per new version.

Resolves #751.


---

_Merged by @charliermarsh on 2022-11-15 21:14_

---

_Closed by @charliermarsh on 2022-11-15 21:14_

---

_Branch deleted on 2022-11-15 21:14_

---
