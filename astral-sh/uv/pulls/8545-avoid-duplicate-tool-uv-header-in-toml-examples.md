```yaml
number: 8545
title: "Avoid duplicate `[tool.uv]` header in TOML examples"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/dup
created_at: 2024-10-24T20:50:18Z
updated_at: 2024-10-24T21:10:02Z
url: https://github.com/astral-sh/uv/pull/8545
synced_at: 2026-01-12T16:08:22Z
```

# Avoid duplicate `[tool.uv]` header in TOML examples

---

_@charliermarsh_

## Summary

For example, in:

```toml
[tool.uv]
[[tool.uv.index]]
name = "pytorch"
url = "https://download.pytorch.org/whl/cu121"
```

We can just omit `[tool.uv]`.


---

_Label `documentation` added by @charliermarsh on 2024-10-24 20:50_

---

_Renamed from "Avoid duplicate [tool.uv] header in TOML examples" to "Avoid duplicate `[tool.uv]` header in TOML examples" by @charliermarsh on 2024-10-24 20:50_

---

_@zanieb approved on 2024-10-24 20:59_

---

_Merged by @charliermarsh on 2024-10-24 21:10_

---

_Closed by @charliermarsh on 2024-10-24 21:10_

---

_Branch deleted on 2024-10-24 21:10_

---
