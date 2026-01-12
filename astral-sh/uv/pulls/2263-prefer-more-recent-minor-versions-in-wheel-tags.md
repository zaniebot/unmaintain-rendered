```yaml
number: 2263
title: Prefer more recent minor versions in wheel tags
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/ver
created_at: 2024-03-07T04:46:25Z
updated_at: 2024-03-07T14:04:56Z
url: https://github.com/astral-sh/uv/pull/2263
synced_at: 2026-01-12T16:04:56Z
```

# Prefer more recent minor versions in wheel tags

---

_@charliermarsh_

## Summary

In the list of tags produced by `Tags::from_env`, higher-priority tags are expected to come earlier in the list. Right now, though, we push tags like `py38` before `py312`. So if you run `cargo run pip install multiprocess -n --reinstall --verbose` on Python 3.12, you get the `py38` wheel rather than the `py32` wheel.

Closes https://github.com/astral-sh/uv/issues/2261.

---

_Label `bug` added by @charliermarsh on 2024-03-07 04:46_

---

_@skshetry approved on 2024-03-07 04:59_

I can confirm that this fixes the issue. Thank you for creating the fix so quickly. ⚡ 

---

_Review requested from @konstin by @charliermarsh on 2024-03-07 05:20_

---

_Merged by @charliermarsh on 2024-03-07 14:04_

---

_Closed by @charliermarsh on 2024-03-07 14:04_

---

_Branch deleted on 2024-03-07 14:04_

---
