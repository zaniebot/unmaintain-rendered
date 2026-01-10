```yaml
number: 4646
title: "Add `uv toolchain uninstall`"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: zb/toolchain-uninstall
created_at: 2024-06-29T15:46:17Z
updated_at: 2024-07-02T02:48:28Z
url: https://github.com/astral-sh/uv/pull/4646
synced_at: 2026-01-10T13:48:28Z
```

# Add `uv toolchain uninstall`

---

_Pull request opened by @zanieb on 2024-06-29 15:46_

_No description provided._

---

_Label `cli` added by @zanieb on 2024-06-29 15:46_

---

_Label `preview` added by @zanieb on 2024-06-29 15:46_

---

_Marked ready for review by @zanieb on 2024-07-02 01:42_

---

_Comment by @zanieb on 2024-07-02 01:42_

We should probably have some sort of safe-guard that checks for usage of the toolchain? But it seems hard to track and not required from the initial implementation.

---

_Review comment by @charliermarsh on `crates/uv/src/main.rs`:900 on 2024-07-02 02:05_

(Extra newline here.)

---

_Review comment by @charliermarsh on `crates/uv/src/commands/toolchain/uninstall.rs`:65 on 2024-07-02 02:08_

Extra `,` here.

---

_Review comment by @charliermarsh on `crates/uv/src/commands/toolchain/uninstall.rs`:93 on 2024-07-02 02:09_

Should we use backticks for the toolchains in the various writes, rather than single quotes?

---

_Review comment by @charliermarsh on `crates/uv/src/commands/toolchain/uninstall.rs`:102 on 2024-07-02 02:09_

Extra `,` here sorry.

---

_@charliermarsh approved on 2024-07-02 02:09_

---

_@zanieb reviewed on 2024-07-02 02:17_

---

_Review comment by @zanieb on `crates/uv/src/commands/toolchain/uninstall.rs`:102 on 2024-07-02 02:17_

I feel failed by my tooling.



---

_Review comment by @zanieb on `crates/uv/src/commands/toolchain/uninstall.rs`:93 on 2024-07-02 02:17_

I have no idea why I wrote it this way previously. I'm happy to change it... I think.

---

_@zanieb reviewed on 2024-07-02 02:17_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/toolchain/uninstall.rs`:102 on 2024-07-02 02:37_

This one annoys me so much (that it's not automatic).

---

_@charliermarsh reviewed on 2024-07-02 02:37_

---

_Merged by @zanieb on 2024-07-02 02:37_

---

_Closed by @zanieb on 2024-07-02 02:37_

---

_Branch deleted on 2024-07-02 02:37_

---

_@zanieb reviewed on 2024-07-02 02:47_

---

_Review comment by @zanieb on `crates/uv/src/commands/toolchain/uninstall.rs`:102 on 2024-07-02 02:47_

Yeah. I'll need to watch for it I guess. Why would anyone want to keep that comma...

---

_Comment by @zanieb on 2024-07-02 02:48_

> We should probably have some sort of safe-guard that checks for usage of the toolchain? But it seems hard to track and not required from the initial implementation.

See #4718 

---
