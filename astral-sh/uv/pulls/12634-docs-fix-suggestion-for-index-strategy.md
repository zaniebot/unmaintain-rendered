```yaml
number: 12634
title: "[docs] Fix suggestion for index strategy"
type: pull_request
state: merged
author: TBoshoven
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-04-02T18:19:57Z
updated_at: 2025-04-02T20:22:28Z
url: https://github.com/astral-sh/uv/pull/12634
synced_at: 2026-01-12T16:10:20Z
```

# [docs] Fix suggestion for index strategy

---

_@TBoshoven_

## Summary

Fix a suggestion in the docs on configs through environment variables, which lists an option value that doesn't appear to exist.
The description implies that `unsafe-best-match` was intended here.

## Test Plan

Verified by providing `unsafe-any-match` as a parameter to `uv`. It didn't error, but appeared to use the `first-index` strategy instead.
The value I changed it to behaves as described by the documentation.

---

_@zanieb approved on 2025-04-02 19:17_

Thanks! Can you change this in the code as well? This file is generated.

See `crates/uv-static/src/env_vars.rs`

---

_Label `documentation` added by @zanieb on 2025-04-02 19:17_

---

_Comment by @TBoshoven on 2025-04-02 19:18_

Ah, I didn't realize these were generated. Will do!

---

_Merged by @zanieb on 2025-04-02 20:22_

---

_Closed by @zanieb on 2025-04-02 20:22_

---
