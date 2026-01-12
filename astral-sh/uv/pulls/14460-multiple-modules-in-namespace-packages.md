```yaml
number: 14460
title: Multiple modules in namespace packages
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - build-backend
assignees: []
merged: true
base: main
head: konsti/multiple-modules
created_at: 2025-07-04T14:33:13Z
updated_at: 2025-07-09T17:45:45Z
url: https://github.com/astral-sh/uv/pull/14460
synced_at: 2026-01-12T16:11:14Z
```

# Multiple modules in namespace packages

---

_@konstin_

Support multiple root modules in namespace packages by enumerating them:

```toml
[tool.uv.build-backend]
module-name = ["foo", "bar"]
```

This allows applications with multiple root packages without migrating to workspaces. Since those are regular module names (we iterate over them an process each one like a single module names), it allows combining dotted (namespace) names and regular names. It also technically allows combining regular and stub modules, even though this is even less recommends.

We don't recommend this structure (please use a workspace instead, or structure everything in one root module), but it reduces the number of cases that need `namespace = true`.

Fixes #14435
Fixes #14438

---

_Label `enhancement` added by @konstin on 2025-07-04 14:33_

---

_Label `build-backend` added by @konstin on 2025-07-04 14:33_

---

_Comment by @chirizxc on 2025-07-05 16:34_

also fixes #14438

---

_Review requested from @BurntSushi by @konstin on 2025-07-07 10:09_

---

_Marked ready for review by @konstin on 2025-07-07 10:09_

---

_@konstin reviewed on 2025-07-07 10:17_

---

_Review comment by @konstin on `docs/concepts/build-backend.md`:151 on 2025-07-07 10:17_

The clearer explanation would be: Only use this for legacy projects, for a new project, use a layout with a single `module-name` value, it'll save you and your user from the pain of debugging this later.

---

_Review requested from @zanieb by @konstin on 2025-07-07 10:17_

---

_@zanieb reviewed on 2025-07-07 20:18_

---

_Review comment by @zanieb on `docs/concepts/build-backend.md`:151 on 2025-07-07 20:18_

I wrote some proposed edits at https://github.com/astral-sh/uv/pull/14494

---

_@zanieb reviewed on 2025-07-07 20:24_

---

_Review comment by @zanieb on `crates/uv-build-backend/src/settings.rs`:38 on 2025-07-07 20:24_

Maybe this should say... "However, we recommend..."?

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/settings.rs`:38 on 2025-07-08 15:52_

Maybe it's beyond the scope of these docs here, but I think it would be useful to expand on the recommendation here. Specifically, saying _why_ we are recommending this and maybe something about the trade-offs here.

---

_@BurntSushi approved on 2025-07-08 15:53_

---

_@konstin reviewed on 2025-07-09 15:33_

---

_Review comment by @konstin on `crates/uv-build-backend/src/settings.rs`:38 on 2025-07-09 15:33_

The general risk with namespaces is that you can have files overwriting each other (this does happen) and it the provenance of modules isn't immediately clear anymore. When using a namespace package `module-name` is recommended over `namespace = true`, since the former checks that the module list is what you intend it to be, while the latter doesn't have that check (see also https://github.com/jwodder/check-wheel-contents, where we cover a number of lints by checking for `__init__.py[i]` when using `module-name` (or the defaults) without `namespace = true`. 

---

_@BurntSushi reviewed on 2025-07-09 15:42_

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/settings.rs`:38 on 2025-07-09 15:42_

Yeah I think if some version of that made it into the docs, that would be great.

---

_Merged by @zanieb on 2025-07-09 17:45_

---

_Closed by @zanieb on 2025-07-09 17:45_

---

_Branch deleted on 2025-07-09 17:45_

---
