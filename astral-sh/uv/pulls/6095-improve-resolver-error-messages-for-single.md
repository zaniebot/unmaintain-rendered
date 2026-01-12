```yaml
number: 6095
title: Improve resolver error messages for single-project workspaces
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
  - preview
assignees: []
merged: true
base: main
head: zb/resolver-errors-projects
created_at: 2024-08-14T22:39:26Z
updated_at: 2024-08-15T03:08:57Z
url: https://github.com/astral-sh/uv/pull/6095
synced_at: 2026-01-12T16:07:12Z
```

# Improve resolver error messages for single-project workspaces

---

_@zanieb_

Extends https://github.com/astral-sh/uv/pull/6092 to improve resolver error messages for workspaces that have a single member.

As before, this requires a two-step approach of

1. Traversing the derivation tree and collapsing some members. In this case, we drop the empty root node in favor of the project.
2. Using special-case formatting for packages. In this case, the workspace package is referred to with "your project" instead of its name.

---

_Label `error messages` added by @zanieb on 2024-08-14 22:39_

---

_Marked ready for review by @zanieb on 2024-08-14 22:50_

---

_Review requested from @charliermarsh by @zanieb on 2024-08-14 22:50_

---

_Label `preview` added by @zanieb on 2024-08-14 22:57_

---

_@zanieb reviewed on 2024-08-14 23:00_

---

_Review comment by @zanieb on `crates/uv-resolver/src/error.rs`:303 on 2024-08-14 23:00_

I initially thought I'd need to convert the dependency nodes from `project -> other` to `root -> other` but it seems like I can get away with not doing that! It depends on some behavior from #6092, otherwise we wouldn't display a proper conclusion but the message I added there about an unsatisfiable project suffices as a conclusion.
  

---

_@charliermarsh reviewed on 2024-08-14 23:58_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/error.rs`:309 on 2024-08-14 23:58_

I'm glad this pattern is working for us.

---

_@charliermarsh approved on 2024-08-14 23:59_

---

_Review comment by @zanieb on `crates/uv-resolver/src/error.rs`:309 on 2024-08-15 00:24_

Yeah, I'd previously had a hard time grokking PubGrub's `collapse_no_versions` so I thought this would be harder but it wasn't too bad and I think it could be a very powerful approach in the future.

---

_@zanieb reviewed on 2024-08-15 00:24_

---

_Merged by @zanieb on 2024-08-15 03:08_

---

_Closed by @zanieb on 2024-08-15 03:08_

---

_Branch deleted on 2024-08-15 03:08_

---
