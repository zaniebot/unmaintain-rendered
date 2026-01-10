```yaml
number: 4164
title: "Add `uv toolchain install`"
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/toolchain-v-ii
created_at: 2024-06-08T16:49:26Z
updated_at: 2024-06-10T15:56:09Z
url: https://github.com/astral-sh/uv/pull/4164
synced_at: 2026-01-10T13:54:02Z
```

# Add `uv toolchain install`

---

_Pull request opened by @zanieb on 2024-06-08 16:49_

Adds a command (following #4163) to download and install specific toolchains. While we fetch toolchains on demand, this is useful for, e.g., pre-downloading a toolchain in a Docker image build.

~I kind of think we should call this `install` instead of `fetch`~ I changed the name from `fetch` to `install`.

---

_Label `preview` added by @zanieb on 2024-06-08 16:49_

---

_Renamed from "Implement `uv toolchain fetch`" to "Implement `uv toolchain install`" by @zanieb on 2024-06-08 17:11_

---

_Renamed from "Implement `uv toolchain install`" to "Add `uv toolchain install`" by @zanieb on 2024-06-09 14:33_

---

_Marked ready for review by @zanieb on 2024-06-10 14:23_

---

_Review requested from @konstin by @zanieb on 2024-06-10 14:37_

---

_Review requested from @charliermarsh by @zanieb on 2024-06-10 14:37_

---

_@charliermarsh approved on 2024-06-10 15:37_

---

_@charliermarsh reviewed on 2024-06-10 15:38_

---

_Review comment by @charliermarsh on `crates/uv-toolchain/src/managed.rs`:227 on 2024-06-10 15:38_

Do you expect a fixed number of parts? You could do `let [implementation, python_version] = parts.as_slice() else { ... }` if so.

---

_Review comment by @zanieb on `crates/uv-toolchain/src/managed.rs`:227 on 2024-06-10 15:38_

Ah yeah totally that'd be nice.

---

_@zanieb reviewed on 2024-06-10 15:38_

---

_@konstin approved on 2024-06-10 15:42_

---

_Merged by @zanieb on 2024-06-10 15:56_

---

_Closed by @zanieb on 2024-06-10 15:56_

---

_Branch deleted on 2024-06-10 15:56_

---
