```yaml
number: 14663
title: Build and install workspace members that are dependencies by default
type: pull_request
state: merged
author: zanieb
labels:
  - breaking
assignees: []
merged: true
base: release/080
head: zb/workspace-default
created_at: 2025-07-16T16:11:43Z
updated_at: 2025-07-17T18:38:04Z
url: https://github.com/astral-sh/uv/pull/14663
synced_at: 2026-01-10T06:53:02Z
```

# Build and install workspace members that are dependencies by default

---

_Pull request opened by @zanieb on 2025-07-16 16:11_

Regardless of the presence of a build system, as in https://github.com/astral-sh/uv/pull/14413


---

_@zanieb reviewed on 2025-07-16 20:11_

---

_Review comment by @zanieb on `crates/uv-resolver/src/lock/mod.rs`:1732 on 2025-07-16 20:11_

We use `seen` since it includes all the recursively collected dependencies.

---

_Marked ready for review by @zanieb on 2025-07-16 20:51_

---

_Label `breaking` added by @zanieb on 2025-07-16 20:51_

---

_Comment by @zanieb on 2025-07-16 21:32_

This seems to have a bad interaction with #14413 now that it's based on it.

---

_@zanieb reviewed on 2025-07-16 22:55_

---

_Review comment by @zanieb on `crates/uv-workspace/src/workspace.rs`:767 on 2025-07-16 22:55_

This isn't strictly correct, as you could have an unused source here? However, if the workspace member is a dependency, you _do_ need the source entry. I'm hesitant to add collection of the full requirement graph here. We might need to consider a different approach.

Unfortunately, I think we need to do something like this (collect the required members when constructing the `Workspace`) because in places like `Workspace:: members_requirements` we don't otherwise have enough information to construct the proper `editable` and `virtual` values. It was "working" before #14197, but somehow that leads to conflicting URL errors now.

---

_@zanieb reviewed on 2025-07-16 22:57_

---

_Review comment by @zanieb on `crates/uv-workspace/src/workspace.rs`:767 on 2025-07-16 22:57_

(You can see the previous approach prior to https://github.com/astral-sh/uv/pull/14663/commits/94efd22fcf3cd64ae02d2a334f1ad37d8c4a476c)

---

_Converted to draft by @zanieb on 2025-07-16 22:57_

---

_@zanieb reviewed on 2025-07-16 22:57_

---

_Review comment by @zanieb on `crates/uv-resolver/src/lock/mod.rs`:1732 on 2025-07-16 22:57_

We're not doing this anymore, see https://github.com/astral-sh/uv/pull/14663#discussion_r2211772923

---

_@zanieb reviewed on 2025-07-17 00:16_

---

_Review comment by @zanieb on `crates/uv-workspace/src/workspace.rs`:767 on 2025-07-17 00:16_

cc @konstin in case you have any ideas here

We could also just not differentiate workspace member build behavior based on whether or not its a dependency, that'd make life way easier.

---

_Review comment by @konstin on `docs/concepts/projects/dependencies.md`:870 on 2025-07-17 14:06_

The backticks look wrong

---

_Review comment by @konstin on `crates/uv/src/commands/project/lock_target.rs`:163 on 2025-07-17 14:29_

nit: Does this need to be static if we clone it?

---

_Review comment by @konstin on `crates/uv-workspace/src/workspace.rs`:402 on 2025-07-17 14:34_

Do we need to / should we filter for `package = false` here?

---

_Marked ready for review by @konstin on 2025-07-17 14:34_

---

_Converted to draft by @konstin on 2025-07-17 14:34_

---

_@konstin reviewed on 2025-07-17 14:34_

---

_@konstin reviewed on 2025-07-17 14:37_

---

_Review comment by @konstin on `crates/uv-workspace/src/workspace.rs`:767 on 2025-07-17 14:37_

I think both this and a proper graph are fine, though I like this one for only looking at the workspace structure and not at the user input, so that whether we build is independent of the specific sync args.

---

_@zanieb reviewed on 2025-07-17 14:47_

---

_Review comment by @zanieb on `crates/uv-workspace/src/workspace.rs`:402 on 2025-07-17 14:47_

`Source::Workspace` doesn't allow `package`

---

_@zanieb reviewed on 2025-07-17 15:51_

---

_Review comment by @zanieb on `crates/uv-workspace/src/workspace.rs`:402 on 2025-07-17 15:51_

It's an open question, I guess, if it needs to?

---

_Review comment by @konstin on `crates/uv-workspace/src/workspace.rs`:402 on 2025-07-17 16:26_

I expect we'll want to add it for symmetry with `path`, but it's not a pressing concern. It's non-breaking to add it from what I can tell, so I'd start without it.

---

_@konstin reviewed on 2025-07-17 16:26_

---

_Marked ready for review by @zanieb on 2025-07-17 18:17_

---

_Merged by @zanieb on 2025-07-17 18:38_

---

_Closed by @zanieb on 2025-07-17 18:38_

---

_Branch deleted on 2025-07-17 18:38_

---
