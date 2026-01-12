```yaml
number: 15347
title: "Add an `aarch64-pc-windows-msvc` target"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/arm-win
created_at: 2025-08-18T13:29:22Z
updated_at: 2025-08-18T17:57:29Z
url: https://github.com/astral-sh/uv/pull/15347
synced_at: 2026-01-12T16:11:42Z
```

# Add an `aarch64-pc-windows-msvc` target

---

_@zanieb_

I needed this and was surprised it didn't exist!

---

_Label `enhancement` added by @zanieb on 2025-08-18 13:29_

---

_Review requested from @konstin by @zanieb on 2025-08-18 14:11_

---

_Review comment by @zanieb on `crates/uv-configuration/src/target_triple.rs`:478 on 2025-08-18 14:20_

I think this is actually wrong. `platform.machine()` reports `ARM64` on Windows, tragically.

---

_@zanieb reviewed on 2025-08-18 14:20_

---

_@zanieb reviewed on 2025-08-18 14:20_

---

_Review comment by @zanieb on `crates/uv-configuration/src/target_triple.rs`:478 on 2025-08-18 14:20_

Does case matter?

---

_Review comment by @konstin on `crates/uv-configuration/src/target_triple.rs`:31 on 2025-08-18 14:53_

```suggestion
    /// An ARM64 Windows target.
```

---

_@konstin approved on 2025-08-18 14:56_

Didn't verify the string values, but code LGTM.

---

_Review comment by @konstin on `crates/uv-configuration/src/target_triple.rs`:478 on 2025-08-18 14:56_

We do string matching with `platform_machine` in PEP 508 and iirc don't case-normalize for it.

---

_@konstin reviewed on 2025-08-18 14:56_

---

_Merged by @zanieb on 2025-08-18 17:57_

---

_Closed by @zanieb on 2025-08-18 17:57_

---

_Branch deleted on 2025-08-18 17:57_

---
