```yaml
number: 12756
title: "Split workspace members onto their own lines in `uv init`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/split
created_at: 2025-04-08T19:33:37Z
updated_at: 2025-04-08T19:51:24Z
url: https://github.com/astral-sh/uv/pull/12756
synced_at: 2026-01-10T11:10:40Z
```

# Split workspace members onto their own lines in `uv init`

---

_Pull request opened by @charliermarsh on 2025-04-08 19:33_

## Summary

See the test cases. Previously, you could end up with something like:

```toml
[tool.uv.workspace]
members = [
    "foo",
    "bar",
    "baz", "bop",
]
```

---

_Label `bug` added by @charliermarsh on 2025-04-08 19:34_

---

_Renamed from "Split workspace members onto their own lines in uv init" to "Split workspace members onto their own lines in `uv init`" by @charliermarsh on 2025-04-08 19:34_

---

_@zanieb approved on 2025-04-08 19:41_

---

_Merged by @charliermarsh on 2025-04-08 19:51_

---

_Closed by @charliermarsh on 2025-04-08 19:51_

---

_Branch deleted on 2025-04-08 19:51_

---
