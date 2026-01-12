```yaml
number: 9411
title: "windows ci: Run `cargo clippy` in the dev drive workspace to reuse the cache"
type: pull_request
state: merged
author: j178
labels:
  - internal
assignees: []
merged: true
base: main
head: clippy
created_at: 2024-11-25T08:22:08Z
updated_at: 2025-03-18T07:53:12Z
url: https://github.com/astral-sh/uv/pull/9411
synced_at: 2026-01-12T16:08:48Z
```

# windows ci: Run `cargo clippy` in the dev drive workspace to reuse the cache

---

_@j178_

## Summary

In the Windows Clippy job, the workspace is transferred to `UV_WORKSPACE`. However, `cargo clippy` continues to execute in the `github.workspace`, and `Swatinem/rust-cache` only caches the `UV_WORKSPACE/target`, resulting in `cargo clippy` having no cache.

 This adjustment will take effect when any changes are made to `Cargo.toml` or `Cargo.lock`, prompting `Swatinem/rust-cache` to updat the cache.



---

_@zanieb approved on 2024-11-25 21:12_

Thanks!

---

_Label `internal` added by @zanieb on 2024-11-25 21:12_

---

_Merged by @zanieb on 2024-11-25 21:12_

---

_Closed by @zanieb on 2024-11-25 21:12_

---

_Branch deleted on 2025-03-18 07:53_

---
