```yaml
number: 5354
title: "Avoid project discovery in `uv python pin` if `--isolated` is provided"
type: pull_request
state: merged
author: j178
labels: []
assignees: []
merged: true
base: main
head: discover
created_at: 2024-07-23T17:38:16Z
updated_at: 2024-07-23T23:47:33Z
url: https://github.com/astral-sh/uv/pull/5354
synced_at: 2026-01-10T13:37:23Z
```

# Avoid project discovery in `uv python pin` if `--isolated` is provided

---

_Pull request opened by @j178 on 2024-07-23 17:38_

## Summary

Instead of discovering and then discarding, try to avoid unnecessary discovery.


---

_Comment by @zanieb on 2024-07-23 17:44_

Thanks! I think I made this change in https://github.com/astral-sh/uv/tree/zb/pin-find too but it seems nice to get it in before #5035 is finished.

---

_@zanieb approved on 2024-07-23 18:07_

---

_Merged by @zanieb on 2024-07-23 18:07_

---

_Closed by @zanieb on 2024-07-23 18:07_

---

_Branch deleted on 2024-07-23 23:47_

---
