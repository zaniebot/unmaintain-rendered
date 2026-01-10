```yaml
number: 15379
title: "Mark `find_uv_bin_py38` test as requiring `python-eol`"
type: pull_request
state: merged
author: mgorny
labels:
  - internal
assignees: []
merged: true
base: main
head: uv-bin-py38
created_at: 2025-08-19T12:54:46Z
updated_at: 2025-08-19T13:06:04Z
url: https://github.com/astral-sh/uv/pull/15379
synced_at: 2026-01-10T06:44:33Z
```

# Mark `find_uv_bin_py38` test as requiring `python-eol`

---

_Pull request opened by @mgorny on 2025-08-19 12:54_

## Summary

Mark `find_uv_bin_py38` test as requiring `python-eol`. Resolves one of the issues reported in #15368.

## Test Plan

```
cargo test --profile=dev --features git --features pypi --features python --no-default-features
```

(without Python 3.8 installed)

---

_Label `internal` added by @konstin on 2025-08-19 13:00_

---

_@konstin approved on 2025-08-19 13:00_

---

_Merged by @konstin on 2025-08-19 13:06_

---

_Closed by @konstin on 2025-08-19 13:06_

---
