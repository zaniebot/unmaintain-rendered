```yaml
number: 3585
title: Discover workspaces without using them in resolution
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/discover-workspaces-without-using-them
created_at: 2024-05-14T18:09:07Z
updated_at: 2024-05-22T09:25:54Z
url: https://github.com/astral-sh/uv/pull/3585
synced_at: 2026-01-12T16:05:43Z
```

# Discover workspaces without using them in resolution

---

_@konstin_

Add minimal support for workspace discovery, only used for determining paths in the bluejay commands.

We can now discover the workspace structure, namely that the `pyproject.toml` of a package belongs to a workspace `pyproject.toml` with members and exclusion. The globbing logic is inspired by cargo. We don't resolve `workspace = true` metadata declarations yet.

---

_Label `preview` added by @konstin on 2024-05-14 18:09_

---

_Review comment by @konstin on `crates/uv-requirements/src/discovery.rs`:40 on 2024-05-14 18:09_

We want this information when resolving workspace dependencies, but we don't use it yet

---

_@konstin reviewed on 2024-05-14 18:09_

---

_@konstin reviewed on 2024-05-14 18:10_

---

_Review comment by @konstin on `crates/uv-requirements/src/discovery.rs`:548 on 2024-05-14 18:10_

I tried the json snapshots here for the first time and find them better than the regular debug snapshots

---

_@konstin reviewed on 2024-05-14 18:11_

---

_Review comment by @konstin on `crates/uv-requirements/src/pyproject.rs`:9 on 2024-05-14 18:11_

This makes the test snapshots deterministic

---

_Review comment by @konstin on `crates/uv-requirements/src/discovery.rs`:1 on 2024-05-15 21:55_

See for a more used state of this file https://gist.github.com/konstin/f8c05311223fbd627b0491295915e865

---

_@konstin reviewed on 2024-05-15 21:55_

---

_@zanieb reviewed on 2024-05-17 01:44_

---

_Review comment by @zanieb on `crates/uv-requirements/src/discovery.rs`:80 on 2024-05-17 01:44_

Does this need a doc too?

---

_@zanieb reviewed on 2024-05-17 01:45_

---

_Review comment by @zanieb on `crates/uv-requirements/src/discovery.rs`:73 on 2024-05-17 01:45_

Can you make this message a bit more user-facing e.g. "Found project root `{}`" or similar?

---

_@zanieb reviewed on 2024-05-17 01:45_

---

_Review comment by @zanieb on `crates/uv-requirements/src/discovery.rs`:208 on 2024-05-17 01:45_

Does this need a doc?

---

_Review comment by @zanieb on `crates/uv-requirements/src/discovery.rs`:68 on 2024-05-17 01:47_

Should we use `is_file()` instead of `exists()`?

---

_@zanieb reviewed on 2024-05-17 01:47_

---

_@zanieb reviewed on 2024-05-17 01:53_

Could you add some documentation explaining what's going on? Or at least link to the relevant "specification" we're implementing here? I'm having a hard time reviewing because there's a lot of behavior in here.

---

_Comment by @konstin on 2024-05-17 11:23_

I've edited the spec from #3404 into a docstring

---

_@charliermarsh reviewed on 2024-05-21 14:50_

If this is causing you trouble, you can merge and I'll do a post-push review.

---

_Comment by @konstin on 2024-05-21 17:05_

Merging this now to avoid merge conflicts

---

_Merged by @konstin on 2024-05-21 17:17_

---

_Closed by @konstin on 2024-05-21 17:17_

---

_Branch deleted on 2024-05-21 17:17_

---

_Comment by @charliermarsh on 2024-05-21 20:53_

@konstin - Gave this a read, looks like a good start.

---

_@charliermarsh reviewed on 2024-05-21 20:54_

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/workspace.rs`:246 on 2024-05-21 20:54_

Should this be an `or_else`?

---

_@charliermarsh reviewed on 2024-05-21 20:55_

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/workspace.rs`:246 on 2024-05-21 20:55_

Does this mean we're in a _member_ of a workspace?

---

_@konstin reviewed on 2024-05-22 09:25_

---

_Review comment by @konstin on `crates/uv-requirements/src/workspace.rs`:246 on 2024-05-22 09:25_

yeah, added some docs in the other branch

---
