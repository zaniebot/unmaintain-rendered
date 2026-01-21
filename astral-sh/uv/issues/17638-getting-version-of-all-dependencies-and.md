```yaml
number: 17638
title: Getting version of all dependencies and transitive dependencies
type: issue
state: closed
author: dbogdoll
labels:
  - question
assignees: []
created_at: 2026-01-21T13:21:36Z
updated_at: 2026-01-21T14:43:02Z
url: https://github.com/astral-sh/uv/issues/17638
synced_at: 2026-01-21T15:05:52Z
```

# Getting version of all dependencies and transitive dependencies

---

_@dbogdoll_

### Question

In my pyproject.toml  I have specified my normal depedencies and dev dependencies.
I would like to list all my normal dependencies, incl. their transitive dependenceis, but not the dev dependencies.
Idealy uv.lock should be queries for this information. 

How would I do that?


### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @dbogdoll on 2026-01-21 13:21_

---

_Comment by @zanieb on 2026-01-21 13:59_

Like `uv export --no-group dev`?

---

_Closed by @dbogdoll on 2026-01-21 14:43_

---
