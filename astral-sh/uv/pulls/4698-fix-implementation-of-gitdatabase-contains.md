```yaml
number: 4698
title: "Fix implementation of `GitDatabase::contains`"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - bug
assignees: []
merged: true
base: main
head: git-bug
created_at: 2024-07-01T16:39:42Z
updated_at: 2024-07-02T21:02:48Z
url: https://github.com/astral-sh/uv/pull/4698
synced_at: 2026-01-12T16:06:24Z
```

# Fix implementation of `GitDatabase::contains`

---

_@ibraheemdev_

## Summary

`GitDatabase::contains` previously only parsed the commit to see if it was a valid hash and didn't verify if the commit existed in the object database. This led to the database never being updated.

Resolves https://github.com/astral-sh/uv/issues/4378.

## Test Plan

Added a test that fails without this change.

---

_@charliermarsh approved on 2024-07-01 16:40_

---

_Label `bug` added by @charliermarsh on 2024-07-01 16:40_

---

_Merged by @ibraheemdev on 2024-07-01 17:01_

---

_Closed by @ibraheemdev on 2024-07-01 17:01_

---

_Branch deleted on 2024-07-01 17:01_

---

_Comment by @zanieb on 2024-07-02 21:02_

@ibraheemdev probably worth using a title that's more descriptive of the effect in the future so we don't need to go read the original issue to write the changelog entry

---
