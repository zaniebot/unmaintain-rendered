```yaml
number: 12432
title: "Update `--prune` help arg from `PRUNE` to `PACKAGE`"
type: pull_request
state: merged
author: nickjj
labels:
  - cli
assignees: []
merged: true
base: main
head: nickjj/update-prune-help-value
created_at: 2025-03-24T15:30:41Z
updated_at: 2025-03-24T15:45:46Z
url: https://github.com/astral-sh/uv/pull/12432
synced_at: 2026-01-12T16:10:17Z
```

# Update `--prune` help arg from `PRUNE` to `PACKAGE`

---

_@nickjj_

## Summary

This fixes https://github.com/astral-sh/uv/issues/12426 which helps use a more accurate arg name in the help output.

## Test Plan

I didn't test it locally, @charliermarsh gave me guidance on what to change so I looked around that file for another example of `value_name` and repeated what I saw. I kept it formatted to 1 line based on it not being a long line. The other example of `value_name` had everything on separate lines because there were a bunch of parameters passed in.


---

_Renamed from "Update --prune help arg from PRUNE to PACKAGE" to "Update `--prune` help arg from `PRUNE` to `PACKAGE`" by @charliermarsh on 2025-03-24 15:31_

---

_Label `cli` added by @charliermarsh on 2025-03-24 15:31_

---

_@charliermarsh approved on 2025-03-24 15:31_

---

_Merged by @charliermarsh on 2025-03-24 15:45_

---

_Closed by @charliermarsh on 2025-03-24 15:45_

---

_Comment by @charliermarsh on 2025-03-24 15:45_

Thanks!

---
