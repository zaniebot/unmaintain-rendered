```yaml
number: 15588
title: Add a plain text backend for credential storage
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: feature/auth
head: zb/auth-plaintext
created_at: 2025-08-30T13:59:38Z
updated_at: 2025-08-30T16:55:19Z
url: https://github.com/astral-sh/uv/pull/15588
synced_at: 2026-01-12T16:11:50Z
```

# Add a plain text backend for credential storage

---

_@zanieb_

Adds a default plain text storage mechanism to `uv auth`.

While we'd prefer to use the system store, the "native" keyring support is experimental still and I don't want to ship an unusable interface. @geofft also suggested that the story for secure credential storage is much weaker on Linux than macOS and Windows and felt this approach would be needed regardless.

We'll switch over to using the native keyring by default in the future. On Linux, we can now fallback to a plaintext store the secret store is not configured, which is a nice property.

Right now, we store credentials in a TOML file in the uv state directory. I expect to also read from the uv config directory in the future, but we don't need it immediately.

---

_Marked ready for review by @zanieb on 2025-08-30 15:40_

---

_@charliermarsh reviewed on 2025-08-30 15:50_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/credentials.rs`:34 on 2025-08-30 15:50_

Make this serde transparent?

---

_@charliermarsh reviewed on 2025-08-30 15:52_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/middleware.rs`:57 on 2025-08-30 15:52_

I'd suggest just importing these rather than using fully-qualified paths.

---

_@zanieb reviewed on 2025-08-30 15:59_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:57 on 2025-08-30 15:59_

(I'm always down for that, that's just claude being weird)

---

_@charliermarsh reviewed on 2025-08-30 16:01_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/store.rs`:181 on 2025-08-30 16:01_

`FxHashMap` everywhere for consistency IMO.

---

_@charliermarsh reviewed on 2025-08-30 16:01_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/store.rs`:192 on 2025-08-30 16:01_

I would just omit this and use `::default`.

---

_Review comment by @charliermarsh on `crates/uv-auth/src/store.rs`:186 on 2025-08-30 16:02_

I think you can just derive `Default`.

---

_@charliermarsh reviewed on 2025-08-30 16:02_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/service.rs`:19 on 2025-08-30 16:04_

Is this right? Shouldn't it be transparent instead?

---

_@charliermarsh reviewed on 2025-08-30 16:04_

---

_Merged by @zanieb on 2025-08-30 16:55_

---

_Closed by @zanieb on 2025-08-30 16:55_

---

_Branch deleted on 2025-08-30 16:55_

---
