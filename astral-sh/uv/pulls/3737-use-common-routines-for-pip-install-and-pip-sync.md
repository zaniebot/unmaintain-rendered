```yaml
number: 3737
title: "Use common routines for `pip install` and `pip sync`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/sync
created_at: 2024-05-22T15:10:01Z
updated_at: 2024-05-22T16:15:18Z
url: https://github.com/astral-sh/uv/pull/3737
synced_at: 2026-01-10T14:32:20Z
```

# Use common routines for `pip install` and `pip sync`

---

_Pull request opened by @charliermarsh on 2024-05-22 15:10_

## Summary

This PR takes the functions used in `pip install`, moves them into a common module, and then replaces all the `pip sync` logic with calls into those functions. The net effect is that `pip install` and `pip sync` share far more code and demonstrate much more consistent behavior.

Closes https://github.com/astral-sh/uv/issues/3555.


---

_Label `internal` added by @charliermarsh on 2024-05-22 15:10_

---

_Comment by @charliermarsh on 2024-05-22 15:12_

The test changes are all in the logging...

Previously, we logged uninstalls in `pip sync` but not `pip install`; now we log in both.

Previously, we logged resolution in `pip install` but not `pip sync`; now we log in both.

I don't feel that strongly about either.


---

_Comment by @charliermarsh on 2024-05-22 15:50_

Ugh, this is surfacing a really subtle bug in a specific case: we have an editable installed, and a user requests a package of the same name, and so we resolve to the already-installed editable during resolution, but it then gets marked as extraneous when we create the install plan.

---

_Merged by @charliermarsh on 2024-05-22 16:15_

---

_Closed by @charliermarsh on 2024-05-22 16:15_

---

_Branch deleted on 2024-05-22 16:15_

---
