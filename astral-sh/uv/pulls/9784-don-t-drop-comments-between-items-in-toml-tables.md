```yaml
number: 9784
title: "Don't drop comments between items in TOML tables"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/comm
created_at: 2024-12-10T19:37:20Z
updated_at: 2024-12-10T19:59:16Z
url: https://github.com/astral-sh/uv/pull/9784
synced_at: 2026-01-10T12:00:01Z
```

# Don't drop comments between items in TOML tables

---

_Pull request opened by @charliermarsh on 2024-12-10 19:37_

## Summary

If you look at Ed's reply [here](https://github.com/toml-rs/toml/issues/818#issuecomment-2532626305), it sounds like we're being too heavy-handed in applying `.fmt()`. I think I added this to handle an issue with inline tables whereby we were inserting a space after a trailing comma? So now I'm just applying `.fmt()` to inline tables, which don't allow comments between elements anyway.

Closes https://github.com/astral-sh/uv/issues/9758.


---

_Label `bug` added by @charliermarsh on 2024-12-10 19:37_

---

_Merged by @charliermarsh on 2024-12-10 19:59_

---

_Closed by @charliermarsh on 2024-12-10 19:59_

---

_Branch deleted on 2024-12-10 19:59_

---
