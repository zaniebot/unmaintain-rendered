```yaml
number: 6383
title: Collapse extras on dev dependencies
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - lock
assignees: []
merged: true
base: main
head: charlie/dev-extra
created_at: 2024-08-21T22:21:26Z
updated_at: 2024-08-21T22:36:52Z
url: https://github.com/astral-sh/uv/pull/6383
synced_at: 2026-01-12T16:07:21Z
```

# Collapse extras on dev dependencies

---

_@charliermarsh_

## Summary

It turns out we weren't applying the collapse logic here, so dev deps with extras were repeated. This was generally ok... unless we ended up _dropping_ an extra, in which case, you now have a duplicate.

Closes https://github.com/astral-sh/uv/issues/6380.


---

_Label `bug` added by @charliermarsh on 2024-08-21 22:21_

---

_Label `lock` added by @charliermarsh on 2024-08-21 22:21_

---

_Merged by @charliermarsh on 2024-08-21 22:36_

---

_Closed by @charliermarsh on 2024-08-21 22:36_

---

_Branch deleted on 2024-08-21 22:36_

---
