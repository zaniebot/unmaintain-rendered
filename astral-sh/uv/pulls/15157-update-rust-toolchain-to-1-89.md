```yaml
number: 15157
title: Update Rust toolchain to 1.89
type: pull_request
state: merged
author: samypr100
labels:
  - internal
assignees: []
merged: true
base: main
head: rust189
created_at: 2025-08-08T03:46:48Z
updated_at: 2025-09-27T22:38:49Z
url: https://github.com/astral-sh/uv/pull/15157
synced_at: 2026-01-12T16:11:36Z
```

# Update Rust toolchain to 1.89

---

_@samypr100_

## Summary

Bumps Rust toolchain to 1.89, but not the MSRV.

Lifetime changes is related to a new lint rule explained in https://blog.rust-lang.org/2025/08/07/Rust-1.89.0/#mismatched-lifetime-syntaxes-lint

## Test Plan

Existing Tests


---

_Comment by @samypr100 on 2025-08-08 03:57_

Interesting, cargo dev and the one test seems to be failing due to a github cdn outage (tag based permalinks are returning 404 in PBS)

---

_@konstin reviewed on 2025-08-08 08:51_

---

_Review comment by @konstin on `crates/uv-static/src/env_vars.rs`:472 on 2025-08-08 08:51_

That's a false positive, PowerShell is just the name.

---

_Label `internal` added by @konstin on 2025-08-08 08:51_

---

_@samypr100 reviewed on 2025-08-08 12:28_

---

_Review comment by @samypr100 on `crates/uv-static/src/env_vars.rs`:472 on 2025-08-08 12:28_

Agreed, I left it as-is as it seemed fine to me nonetheless. Would you prefer to change it back?

---

_Marked ready for review by @samypr100 on 2025-08-08 12:41_

---

_@zanieb reviewed on 2025-08-08 12:43_

---

_Review comment by @zanieb on `crates/uv-static/src/env_vars.rs`:472 on 2025-08-08 12:43_

I'd update https://github.com/astral-sh/uv/blob/56266447e228d7c042036f26e86f4fe4f46d66f2/clippy.toml#L1-L12

---

_@samypr100 reviewed on 2025-08-08 12:51_

---

_Review comment by @samypr100 on `crates/uv-static/src/env_vars.rs`:472 on 2025-08-08 12:51_

Done in [ref](https://github.com/astral-sh/uv/compare/9b1688b1fed771e0ccef6009e8b2f61d0aa1a834..376c13e783b5337edb0a4590fd4b5c307d5e38c5)

---

_@konstin approved on 2025-08-08 12:52_

---

_Merged by @konstin on 2025-08-08 13:01_

---

_Closed by @konstin on 2025-08-08 13:01_

---

_Branch deleted on 2025-09-27 22:38_

---
