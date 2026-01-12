```yaml
number: 9171
title: termux install python not found cpython-3.12.7-linux-aarch64-none
type: issue
state: closed
author: WangZhongDian
labels: []
assignees: []
created_at: 2024-11-17T10:35:13Z
updated_at: 2024-11-18T01:21:08Z
url: https://github.com/astral-sh/uv/issues/9171
synced_at: 2026-01-12T15:59:44Z
```

# termux install python not found cpython-3.12.7-linux-aarch64-none

---

_@WangZhongDian_

I use `uv python list -- all-platforms` and do not have `cpython-3.12.7-linux-arch64-none`. I am using Android's termux for UV usage。Hope to add `cpython-3.12.7-linux-arch64-none` build

---

_Comment by @charliermarsh on 2024-11-18 01:20_

My guess is that it's related to: https://github.com/astral-sh/uv/pull/9005.

---

_Comment by @charliermarsh on 2024-11-18 01:21_

I think we should track this in https://github.com/astral-sh/uv/issues/2408.

---

_Closed by @charliermarsh on 2024-11-18 01:21_

---
