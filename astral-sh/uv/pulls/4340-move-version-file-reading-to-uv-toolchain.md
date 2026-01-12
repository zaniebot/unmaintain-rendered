```yaml
number: 4340
title: "Move version file reading to `uv-toolchain`"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/toolchain-version-files
created_at: 2024-06-16T15:13:48Z
updated_at: 2024-06-17T18:54:24Z
url: https://github.com/astral-sh/uv/pull/4340
synced_at: 2026-01-12T16:06:10Z
```

# Move version file reading to `uv-toolchain`

---

_@zanieb_

In preparation for using this in other commands.

---

_Label `internal` added by @zanieb on 2024-06-16 15:13_

---

_Review comment by @konstin on `crates/uv-toolchain/src/version_files.rs`:11 on 2024-06-17 08:57_

Should those be methods on `ToolchainRequest`?

---

_@konstin approved on 2024-06-17 08:57_

---

_@zanieb reviewed on 2024-06-17 16:38_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/version_files.rs`:11 on 2024-06-17 16:38_

That's a good question, I guess they could be? It seems a little weird to me to return `Vec<Self>`?

---

_@zanieb reviewed on 2024-06-17 16:40_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/version_files.rs`:11 on 2024-06-17 16:40_

I guess it could be `all_from_version_file` and `from_version_file`?

---

_Review requested from @BurntSushi by @zanieb on 2024-06-17 16:58_

---

_Review comment by @BurntSushi on `crates/uv-toolchain/src/version_files.rs`:11 on 2024-06-17 17:36_

I could go either way personally.

Does this need to be `Option<Vec<T>>` though? Can we just use `Vec<T>`? I think the former is necessary if you specifically need to distinguish between "not specified" and "empty sequence."

---

_@BurntSushi approved on 2024-06-17 17:36_

---

_@zanieb reviewed on 2024-06-17 18:42_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/version_files.rs`:11 on 2024-06-17 18:42_

We want to distinguish between the file does not exist and the file exists but is empty.

---

_@zanieb reviewed on 2024-06-17 18:43_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/version_files.rs`:11 on 2024-06-17 18:43_

I'll save moving these methods for after the stack is merged (if it makes sense) to save myself some rebasing.

---

_Merged by @zanieb on 2024-06-17 18:54_

---

_Closed by @zanieb on 2024-06-17 18:54_

---

_Branch deleted on 2024-06-17 18:54_

---
