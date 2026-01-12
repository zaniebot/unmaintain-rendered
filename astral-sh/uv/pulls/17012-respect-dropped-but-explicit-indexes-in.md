```yaml
number: 17012
title: Respect dropped (but explicit) indexes in dependency groups
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/d
created_at: 2025-12-06T03:05:56Z
updated_at: 2025-12-06T14:06:47Z
url: https://github.com/astral-sh/uv/pull/17012
synced_at: 2026-01-12T16:12:33Z
```

# Respect dropped (but explicit) indexes in dependency groups

---

_@charliermarsh_

## Summary

There are a class of outcomes whereby an index might not be included in "allowed indexes", but could still correctly appear in a lockfile. In the linked case, we have two `default = true` indexes, and one of them is also named. We omit the second `default = true` index from the list of "allowed indexes", but since it's named, a dependency can reference it explicitly. We handle this correctly for `project.dependencies`, but the handling was incorrectly omitting dependency groups.

Closes https://github.com/astral-sh/uv/issues/16843.


---

_Label `bug` added by @charliermarsh on 2025-12-06 03:13_

---

_Marked ready for review by @charliermarsh on 2025-12-06 03:13_

---

_Merged by @charliermarsh on 2025-12-06 14:06_

---

_Closed by @charliermarsh on 2025-12-06 14:06_

---

_Branch deleted on 2025-12-06 14:06_

---
