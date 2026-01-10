```yaml
number: 2918
title: "Improve operating system and architecture types in `uv-toolchain`"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: zb/uv-toolchain
head: zb/uv-toolchain-platform
created_at: 2024-04-08T23:34:33Z
updated_at: 2024-04-09T16:51:25Z
url: https://github.com/astral-sh/uv/pull/2918
synced_at: 2026-01-10T14:43:31Z
```

# Improve operating system and architecture types in `uv-toolchain`

---

_Pull request opened by @zanieb on 2024-04-08 23:34_

Prompted in https://github.com/astral-sh/uv/pull/2842#discussion_r1555640300

Updates the toolchain `Os` and `Arch` types to be a closer match to those in `platform-tags`.

They don't match exactly, e.g. we track `Libc` separately instead of using `Manylinux` and `Musllinux` and we don't need release versions. This makes me think we might not actually want to consolidate to central shared types.

---

_Label `internal` added by @zanieb on 2024-04-08 23:34_

---

_@zanieb reviewed on 2024-04-08 23:36_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/find.rs`:5 on 2024-04-08 23:36_

This is unrelated but was annoying to remove.

This module will be used for existing toolchain lookups in the next pull request. 

---

_Review requested from @konstin by @zanieb on 2024-04-08 23:46_

---

_Review comment by @konstin on `crates/uv-dev/src/fetch_python.rs`:3 on 2024-04-09 09:18_

nit: The import formatting is different from what we usually do

---

_Review comment by @konstin on `crates/uv-toolchain/Cargo.toml`:29 on 2024-04-09 09:18_

table needs sorting

---

_Review comment by @konstin on `crates/uv-toolchain/fetch-version-metadata.py`:232 on 2024-04-09 09:26_

Could you add some comments how the sorting works?

---

_@konstin approved on 2024-04-09 09:26_

---

_@zanieb reviewed on 2024-04-09 13:38_

---

_Review comment by @zanieb on `crates/uv-dev/src/fetch_python.rs`:3 on 2024-04-09 13:38_

Is there a way we can automate this? This is just how rust-analyzer imports the names.

---

_@konstin reviewed on 2024-04-09 14:07_

---

_Review comment by @konstin on `crates/uv-dev/src/fetch_python.rs`:3 on 2024-04-09 14:07_

iirc the problem was that this is nightly option in rustfmt, charlie and i are using the jetbrains action. Just removing the newlines should give a reasonable style, rsutfix is a bit annoying because it removes the import but leaves a newline behind.

---

_@zanieb reviewed on 2024-04-09 14:53_

---

_Review comment by @zanieb on `crates/uv-toolchain/fetch-version-metadata.py`:232 on 2024-04-09 14:53_

Honestly might want to move all this sorting elsewhere.... thanks for raising.

---

_Marked ready for review by @zanieb on 2024-04-09 16:43_

---

_@zanieb reviewed on 2024-04-09 16:45_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/find.rs`:5 on 2024-04-09 16:45_

See #2931 

---

_Comment by @zanieb on 2024-04-09 16:51_

Merging into #2842.

---

_Merged by @zanieb on 2024-04-09 16:51_

---

_Closed by @zanieb on 2024-04-09 16:51_

---

_Branch deleted on 2024-04-09 16:51_

---
