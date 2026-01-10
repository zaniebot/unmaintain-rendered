```yaml
number: 5502
title: "Error when multiple git references are provided in `uv add`"
type: pull_request
state: merged
author: eth3lbert
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: add-group-gitref
created_at: 2024-07-27T01:23:45Z
updated_at: 2024-07-30T14:48:51Z
url: https://github.com/astral-sh/uv/pull/5502
synced_at: 2026-01-10T13:37:23Z
```

# Error when multiple git references are provided in `uv add`

---

_Pull request opened by @eth3lbert on 2024-07-27 01:23_

## Summary

This PR allows us to reject specifying more than one git-ref at the cli level.

## Test Plan

Test cases included.


---

_Renamed from "Add git-ref group for uv add" to "Add `git-ref` group for `uv add`" by @eth3lbert on 2024-07-27 01:30_

---

_Assigned to @zanieb by @zanieb on 2024-07-30 14:35_

---

_Comment by @zanieb on 2024-07-30 14:40_

I wonder if there's a use-case where we'd want to allow multiple to be passed and just confirm they resolve to consistent values? Seems weird though.

---

_@zanieb approved on 2024-07-30 14:40_

---

_Label `cli` added by @zanieb on 2024-07-30 14:40_

---

_Label `preview` added by @zanieb on 2024-07-30 14:40_

---

_Merged by @zanieb on 2024-07-30 14:40_

---

_Closed by @zanieb on 2024-07-30 14:40_

---

_Renamed from "Add `git-ref` group for `uv add`" to "Error when multiple git references are provided in `uv add`" by @zanieb on 2024-07-30 14:41_

---

_Comment by @eth3lbert on 2024-07-30 14:48_

> I wonder if there's a use-case where we'd want to allow multiple to be passed and just confirm they resolve to consistent values? Seems weird though.

I think that would cause more trouble than it's worth, and might consider using `rev` instead. 😅 

---

_Branch deleted on 2024-07-30 14:48_

---
