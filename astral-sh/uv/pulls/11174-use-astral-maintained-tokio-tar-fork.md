```yaml
number: 11174
title: "Use Astral-maintained `tokio-tar` fork"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
  - security
assignees: []
merged: true
base: main
head: charlie/mark
created_at: 2025-02-03T01:02:43Z
updated_at: 2025-02-03T17:51:36Z
url: https://github.com/astral-sh/uv/pull/11174
synced_at: 2026-01-12T16:09:43Z
```

# Use Astral-maintained `tokio-tar` fork

---

_@charliermarsh_

## Summary

I shipped one security fix here along with several significant performance improvements for large TAR files:

- https://github.com/astral-sh/tokio-tar/pull/2
- https://github.com/astral-sh/tokio-tar/pull/4
- https://github.com/astral-sh/tokio-tar/pull/5

I also PR'd the security fix to `edera-dev` (https://github.com/edera-dev/tokio-tar/pull/4).


---

_Label `performance` added by @charliermarsh on 2025-02-03 01:02_

---

_Label `security` added by @charliermarsh on 2025-02-03 01:02_

---

_Review requested from @Gankra by @charliermarsh on 2025-02-03 01:02_

---

_Review requested from @zanieb by @charliermarsh on 2025-02-03 01:02_

---

_Review requested from @konstin by @charliermarsh on 2025-02-03 01:02_

---

_Converted to draft by @charliermarsh on 2025-02-03 01:34_

---

_Comment by @charliermarsh on 2025-02-03 01:34_

Converting to draft while I checkout failing debug assertions.

---

_Review comment by @charliermarsh on `crates/uv-extract/src/stream.rs`:155 on 2025-02-03 01:48_

This is related to https://github.com/astral-sh/tokio-tar/pull/2. I forgot that we don't actually use the crate's `unpack` method, we unpack ourselves (since we need to set permissions in a different way).

---

_@charliermarsh reviewed on 2025-02-03 01:48_

---

_@charliermarsh reviewed on 2025-02-03 01:48_

---

_Review comment by @charliermarsh on `crates/uv-extract/src/stream.rs`:175 on 2025-02-03 01:48_

We can get rid of this whole vendored `unpacked_at` implementation. It turns out the crate now returns the target path. (That's not a change I made, it must've been made in one of the forks.)

---

_Marked ready for review by @charliermarsh on 2025-02-03 01:56_

---

_Review comment by @konstin on `crates/uv-extract/src/stream.rs`:154 on 2025-02-03 12:38_

Is this a concern for us? We usually create directories from archives without preserving permissions (at least in the zip path we should).

---

_@konstin approved on 2025-02-03 12:38_

---

_@Gankra reviewed on 2025-02-03 14:16_

---

_Review comment by @Gankra on `crates/uv-extract/src/stream.rs`:154 on 2025-02-03 14:16_

Also maybe this is me being unfamiliar with the codebase but this is so... bizarre. How does this unpacking a file before the dir that contains it make sense? Is the idea that we create_dir_all any file path anyway, so the dir entries only create empty dirs and set perms on dirs that have files?

---

_@Gankra approved on 2025-02-03 14:17_

---

_@konstin reviewed on 2025-02-03 14:21_

---

_Review comment by @konstin on `crates/uv-extract/src/stream.rs`:154 on 2025-02-03 14:21_

For zips we always create the parent of a file, as a zip can contain directory entries, but sometimes there are no directory entries, just files.

https://github.com/astral-sh/uv/blob/4d3809cc6b28d71f26a782929b994abfebd7973c/crates/uv-extract/src/stream.rs#L66-L76

---

_@charliermarsh reviewed on 2025-02-03 15:37_

---

_Review comment by @charliermarsh on `crates/uv-extract/src/stream.rs`:154 on 2025-02-03 15:37_

> Is the idea that we create_dir_all any file path anyway, so the dir entries only create empty dirs and set perms on dirs that have files?

Yes. When we create a file, we create the directories required for it. This happens within the `async-tar` crate. The reason we have to do it "manually" here is because we have our own `unpack` that replicates the `async-tar` logic but applies different permissions.


---

_@charliermarsh reviewed on 2025-02-03 15:40_

---

_Review comment by @charliermarsh on `crates/uv-extract/src/stream.rs`:154 on 2025-02-03 15:40_

(I will look at why the `tar` crate introduced this before merging.)

---

_@charliermarsh reviewed on 2025-02-03 15:56_

---

_Review comment by @charliermarsh on `crates/uv-extract/src/stream.rs`:154 on 2025-02-03 15:56_

It kind of seems like a bug that we aren't preserving directory permissions in the zip case?

---

_@konstin reviewed on 2025-02-03 16:10_

---

_Review comment by @konstin on `crates/uv-extract/src/stream.rs`:154 on 2025-02-03 16:10_

personally, i'd prefer only preserving the executable bit and otherwise using the default umask; i'd find it odd if the permissions on the build host should determine the permissions in the deployment target.

---

_Review comment by @charliermarsh on `crates/uv-extract/src/stream.rs`:154 on 2025-02-03 16:19_

Yeah. Probably requires more invasive changes to the `tar` crate though.

---

_@charliermarsh reviewed on 2025-02-03 16:19_

---

_Review comment by @charliermarsh on `crates/uv-extract/src/stream.rs`:154 on 2025-02-03 16:36_

I'd like to review what pip does here before making other changes (since this is just preserving our status quo).

---

_@charliermarsh reviewed on 2025-02-03 16:36_

---

_@charliermarsh reviewed on 2025-02-03 17:48_

---

_Review comment by @charliermarsh on `crates/uv-extract/src/stream.rs`:154 on 2025-02-03 17:48_

Nevermind, pip explicitly only preserves the executable bit. I'll make the same change for tar in a separate PR.

https://github.com/pypa/pip/blob/3898741e29b7279e7bffe044ecfbe20f6a438b1e/src/pip/_internal/utils/unpacking.py#L88-L100

---

_Merged by @charliermarsh on 2025-02-03 17:51_

---

_Closed by @charliermarsh on 2025-02-03 17:51_

---

_Branch deleted on 2025-02-03 17:51_

---
