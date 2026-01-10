```yaml
number: 4208
title: Always install as editable when duplicates are requested
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/ed
created_at: 2024-06-10T18:40:39Z
updated_at: 2024-06-10T19:02:18Z
url: https://github.com/astral-sh/uv/pull/4208
synced_at: 2026-01-10T13:54:02Z
```

# Always install as editable when duplicates are requested

---

_Pull request opened by @charliermarsh on 2024-06-10 18:40_

## Summary

If the user requests a package as both editable and non-editable, the editable now "wins".

Previously, `pip install -e . .` would install as editable. However, `pip install -e . -r requirements.txt` would _not_ if `requirements.txt` contained `.`, because we ignored `editable` when deduplicating and the order of iteration was just dependent on internals.

Closes https://github.com/astral-sh/uv/issues/4053.


---

_Label `bug` added by @charliermarsh on 2024-06-10 18:40_

---

_Merged by @charliermarsh on 2024-06-10 19:02_

---

_Closed by @charliermarsh on 2024-06-10 19:02_

---

_Branch deleted on 2024-06-10 19:02_

---
