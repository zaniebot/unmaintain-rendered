```yaml
number: 14683
title: Build workspace member graph to determine if members are dependencies
type: pull_request
state: closed
author: jtfmumm
labels:
  - do-not-merge
assignees: []
draft: true
base: release/080
head: jtfm/workspace-graph
created_at: 2025-07-17T15:33:30Z
updated_at: 2025-07-17T20:33:53Z
url: https://github.com/astral-sh/uv/pull/14683
synced_at: 2026-01-10T06:53:02Z
```

# Build workspace member graph to determine if members are dependencies

---

_Pull request opened by @jtfmumm on 2025-07-17 15:33_

This draft PR contains a first pass at building a graph of workspace member dependencies to differentiate members that are graph sources from members that are dependencies. This is for checking whether we should build a member by default (see #14663).


---

_Label `do-not-merge` added by @jtfmumm on 2025-07-17 15:33_

---

_@zanieb reviewed on 2025-07-17 15:38_

---

_Review comment by @zanieb on `crates/uv-workspace/src/workspace.rs`:1648 on 2025-07-17 15:38_

You need to handle optional dependencies and dependency groups, right?

---

_@zanieb reviewed on 2025-07-17 15:40_

---

_Review comment by @zanieb on `crates/uv-workspace/src/workspace.rs`:1680 on 2025-07-17 15:40_

Isn't this type annotation redundant given the return signature?

---

_@zanieb reviewed on 2025-07-17 15:41_

---

_Review comment by @zanieb on `crates/uv-workspace/src/workspace.rs`:1639 on 2025-07-17 15:41_

What is the graph getting us here? Why can't we just insert directly into a `BTreeMap`?

---

_@zanieb reviewed on 2025-07-17 15:41_

---

_Review comment by @zanieb on `crates/uv-workspace/src/workspace.rs`:1651 on 2025-07-17 15:41_

I think we're going to want to omit self dependencies, e.g., for `add_self`

---

_@zanieb reviewed on 2025-07-17 15:43_

---

_Review comment by @zanieb on `crates/uv-workspace/src/workspace.rs`:966 on 2025-07-17 15:43_

nit: It's a little weird to have `project.project`, e.g., to access the inner `Project`. It hints the naming might not be quite right.


---

_@zanieb reviewed on 2025-07-17 15:44_

---

_Review comment by @zanieb on `crates/uv-workspace/src/workspace.rs`:951 on 2025-07-17 15:44_

This type only exists for `collect_members_only`, right? Could we just declare this type locally and keep the public `WorkspaceMember` flat?

---

_@zanieb reviewed on 2025-07-17 15:47_

---

_Review comment by @zanieb on `crates/uv-workspace/src/workspace.rs`:966 on 2025-07-17 15:47_

Would be resolved by https://github.com/astral-sh/uv/pull/14683#discussion_r2213705998

---

_Closed by @jtfmumm on 2025-07-17 20:33_

---
