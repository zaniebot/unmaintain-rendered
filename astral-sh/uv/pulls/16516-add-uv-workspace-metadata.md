```yaml
number: 16516
title: "Add `uv workspace metadata`"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zb/metadata
created_at: 2025-10-30T16:05:06Z
updated_at: 2025-11-11T15:46:03Z
url: https://github.com/astral-sh/uv/pull/16516
synced_at: 2026-01-10T06:28:12Z
```

# Add `uv workspace metadata`

---

_Pull request opened by @zanieb on 2025-10-30 16:05_

This adds the scaffolding for a `uv workspace metadata` command, as an equivalent to `cargo metadata`, for integration with downstream tools. I didn't do much here beyond emit the workspace root path and the paths of the workspace members. I explored doing a bit more in #16638, but I think we're actually going to want to come up with a fairly comprehensive schema like `cargo metadata` has. I've started exploring that too, but I don't have a concrete proposal to share yet.

I don't want this to be a top-level command because I think people would expect `uv metadata <PACKAGE>` to show metadata about arbitrary packages (this has been requested several times). I also think we can do other things in the workspace namespace to make trivial integrations simpler, like `uv workspace list` (enumerate members) and `uv workspace dir` (show the path to the workspace root).

I don't expect this to be stable at all to start. I've both gated it with preview and hidden it from the help. The intent is to merge so we can iterate on it as we figure out what integrations need.

---

_Comment by @elephantum on 2025-11-07 16:41_

I was looking exactly for this sort of functionality today.

It would be great if alongside with names of workspace packages, this command could return their dependencies (I'm specifically interested in dependency relations between workspace members)

---

_Comment by @zanieb on 2025-11-07 16:46_

I actually have that implemented locally — I need the exact same feature :) it's just more complicated so I was going to open a separate pull request.

---

_Marked ready for review by @zanieb on 2025-11-10 22:33_

---

_Label `cli` added by @zanieb on 2025-11-10 23:22_

---

_@charliermarsh reviewed on 2025-11-11 00:23_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/workspace/metadata.rs`:52 on 2025-11-11 00:23_

Project metadata, maybe?

---

_@charliermarsh reviewed on 2025-11-11 00:23_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/workspace/metadata.rs`:52 on 2025-11-11 00:23_

Or, "workspace metadata"?

---

_@charliermarsh approved on 2025-11-11 00:24_

---

_@konstin approved on 2025-11-11 15:08_

---

_@zanieb reviewed on 2025-11-11 15:18_

---

_Review comment by @zanieb on `crates/uv/src/commands/workspace/metadata.rs`:52 on 2025-11-11 15:18_

Shameful AI slop, thanks!

---

_Merged by @zanieb on 2025-11-11 15:46_

---

_Closed by @zanieb on 2025-11-11 15:46_

---

_Branch deleted on 2025-11-11 15:46_

---
