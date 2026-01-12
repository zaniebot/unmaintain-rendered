```yaml
number: 7779
title: Playground release not working in automated runs 
type: issue
state: closed
author: zanieb
labels:
  - release
assignees: []
created_at: 2023-10-03T13:42:59Z
updated_at: 2023-10-03T19:33:10Z
url: https://github.com/astral-sh/ruff/issues/7779
synced_at: 2026-01-12T15:54:47Z
```

# Playground release not working in automated runs 

---

_@zanieb_

The playground release ran after `v0.0.292` and succeeded but the production version was not updated: https://github.com/astral-sh/ruff/actions/runs/6383308175

A manual run of the release workflow resolved this: https://github.com/astral-sh/ruff/actions/runs/6393703232/job/17353483270

It looks like this happened for `v0.0.291` as well. This looks similar to the bug we had with documentation deploys at https://github.com/astral-sh/ruff/issues/7276 and was reported by a user in #7776 

---

_Label `release` added by @zanieb on 2023-10-03 13:43_

---

_Assigned to @zanieb by @zanieb on 2023-10-03 13:44_

---

_Closed by @zanieb on 2023-10-03 19:33_

---
