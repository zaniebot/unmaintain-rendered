```yaml
number: 5231
title: "Don't apply irrelevant constraints when validating site-packages"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/constraints
created_at: 2024-07-19T20:40:46Z
updated_at: 2024-07-19T20:49:23Z
url: https://github.com/astral-sh/uv/pull/5231
synced_at: 2026-01-12T16:06:42Z
```

# Don't apply irrelevant constraints when validating site-packages

---

_@charliermarsh_

## Summary

The current code was checking every constraint against every requirement, regardless of whether they were applicable. In general, this isn't a big deal, because this method is only used as a fast-path to skip resolution -- so we just had way more false-negatives than we should've when constraints were applied. But it's clearly wrong :)

## Test Plan

- `uv venv`
- `uv pip install flask`
- `uv pip install --verbose flask -c constraints.txt` (with `numpy<1.0`)

Prior to this change, Flask was reported as not satisfied.


---

_Review requested from @zanieb by @charliermarsh on 2024-07-19 20:40_

---

_Marked ready for review by @charliermarsh on 2024-07-19 20:40_

---

_Label `bug` added by @charliermarsh on 2024-07-19 20:40_

---

_Comment by @charliermarsh on 2024-07-19 20:41_

Hard to test because the output logging really isn't any different.

---

_@zanieb approved on 2024-07-19 20:47_

---

_Merged by @charliermarsh on 2024-07-19 20:49_

---

_Closed by @charliermarsh on 2024-07-19 20:49_

---

_Branch deleted on 2024-07-19 20:49_

---
