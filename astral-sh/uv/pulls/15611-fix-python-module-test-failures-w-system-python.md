```yaml
number: 15611
title: "Fix `python_module` test failures w/ system Python and installed uv"
type: pull_request
state: merged
author: mgorny
labels: []
assignees: []
merged: true
base: main
head: python-module-test-prefix
created_at: 2025-08-31T19:11:29Z
updated_at: 2025-09-02T14:03:42Z
url: https://github.com/astral-sh/uv/pull/15611
synced_at: 2026-01-12T16:11:51Z
```

# Fix `python_module` test failures w/ system Python and installed uv

---

_@mgorny_

## Summary

Override `sys.base_prefix` when performing `python_module` tests, in order to prevent `find_uv_bin()` from finding `uv` installed alongside system Python, and therefore fix test failures on Gentoo.

Fixes #15368

## Test Plan

```
cargo test --profile=fast-build --features git --features pypi --features python --no-default-features --test it python_module
```

---

_Assigned to @zanieb by @zanieb on 2025-08-31 19:38_

---

_@zanieb approved on 2025-09-02 13:45_

---

_Merged by @zanieb on 2025-09-02 13:45_

---

_Closed by @zanieb on 2025-09-02 13:45_

---

_Comment by @mgorny on 2025-09-02 14:03_

Thanks!

---

_Branch deleted on 2025-09-02 14:03_

---
