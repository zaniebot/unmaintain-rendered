```yaml
number: 8309
title: Refactor development dependency configuration
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: tracking/735
head: zb/dev-refactor
created_at: 2024-10-17T21:41:57Z
updated_at: 2024-10-18T16:58:01Z
url: https://github.com/astral-sh/uv/pull/8309
synced_at: 2026-01-12T16:08:15Z
```

# Refactor development dependency configuration

---

_@zanieb_

Part of #8090 
Unblocks https://github.com/astral-sh/uv/pull/8274

Refactors `DevMode` and `DevSpecification` into a shared type `DevGroupsSpecification` that allows us to track if `--dev` was implicitly or explicitly provided.

---

_Label `internal` added by @zanieb on 2024-10-17 21:41_

---

_Marked ready for review by @zanieb on 2024-10-17 21:52_

---

_Review requested from @charliermarsh by @zanieb on 2024-10-18 14:36_

---

_Review comment by @zanieb on `crates/uv-configuration/src/dev.rs`:119 on 2024-10-18 14:37_

This will become `with_default_groups` or something in the future.

---

_@zanieb reviewed on 2024-10-18 14:37_

---

_@charliermarsh reviewed on 2024-10-18 16:39_

---

_Review comment by @charliermarsh on `crates/uv-configuration/Cargo.toml`:29 on 2024-10-18 16:39_

Can this be removed? I don't see a usage. (I'd prefer to avoid it outside of the `uv` crate itself if possible.)

---

_@charliermarsh approved on 2024-10-18 16:39_

Nice.

---

_@zanieb reviewed on 2024-10-18 16:46_

---

_Review comment by @zanieb on `crates/uv-configuration/Cargo.toml`:29 on 2024-10-18 16:46_

Yeah, I was using it but changed to `unreachable`

---

_Merged by @zanieb on 2024-10-18 16:57_

---

_Closed by @zanieb on 2024-10-18 16:57_

---

_Branch deleted on 2024-10-18 16:58_

---
