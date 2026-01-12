```yaml
number: 6619
title: Clarify package priority order in pip compatibility guide
type: pull_request
state: merged
author: notatallshaw
labels:
  - documentation
assignees: []
merged: true
base: main
head: clarify-package-priority-order
created_at: 2024-08-25T21:35:39Z
updated_at: 2024-08-27T00:07:45Z
url: https://github.com/astral-sh/uv/pull/6619
synced_at: 2026-01-12T16:07:27Z
```

# Clarify package priority order in pip compatibility guide

---

_@notatallshaw_

This is a minor documentation update to a recently added section "Package priority" in the pip compatibility guide. The aim of this PR is clear up two things which I think the current paragraph implies but I don't think are (always) true:

1. That pip doesn't use provided order to prioritize resolution
2. That uv relies solely on provided order to prioritize resolution

What is true, at least for now, is pip has more heuristics than uv to prioritize during resolution, and so I've tried to rework this to make it clear why changing the order might help uv come to a different resolution whereas for pip it might not make a difference.


---

_Comment by @charliermarsh on 2024-08-26 02:15_

Could we perhaps clarify that uv does use _some_ other prioritization? E.g., direct URLs and exactly-pinned versions are solved first? (Same is true of pip, pip just has a few more.)

---

_Label `documentation` added by @charliermarsh on 2024-08-26 02:15_

---

_Assigned to @charliermarsh by @zanieb on 2024-08-26 17:28_

---

_Comment by @notatallshaw on 2024-08-27 00:07_

> Could we perhaps clarify that uv does use _some_ other prioritization?

I thought I was clarifying that, I've updated the language to be more assertive.

---

_@charliermarsh approved on 2024-08-27 00:07_

Thanks!

---

_Merged by @charliermarsh on 2024-08-27 00:07_

---

_Closed by @charliermarsh on 2024-08-27 00:07_

---
