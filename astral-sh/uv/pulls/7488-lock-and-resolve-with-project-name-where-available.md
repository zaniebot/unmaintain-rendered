```yaml
number: 7488
title: Lock and resolve with project name where available
type: pull_request
state: closed
author: lucab
labels: []
assignees: []
draft: true
base: main
head: ups/lock-with-project-name
created_at: 2024-09-18T09:40:13Z
updated_at: 2024-09-20T10:34:06Z
url: https://github.com/astral-sh/uv/pull/7488
synced_at: 2026-01-12T16:07:51Z
```

# Lock and resolve with project name where available

---

_@lucab_

This populates the project package name (if available) for lock-and-resolve operations.


---

_Review comment by @lucab on `crates/uv/src/commands/project/lock.rs`:535 on 2024-09-18 09:44_

Note: drive-by fix, I noticed this detail was missing when looking into a derivation tree result from PubGrub. I *think* in practice this should be a no-op change for any observable behavior, but I'll let CI check that.

---

_@lucab reviewed on 2024-09-18 09:44_

---

_Marked ready for review by @lucab on 2024-09-18 12:26_

---

_Comment by @zanieb on 2024-09-18 12:50_

I believe the PubGrub "root" package is intentionally `None` when you're working in a project. Instead, the root has a dependency on all of the workspace members. This modeling is important for the resolver in some ways I don't fully understand, but we workaround this in the error reports:

https://github.com/astral-sh/uv/blob/5e1b9b196495aea2f0f39fdd4a016471c440bba7/crates/uv-resolver/src/pubgrub/report.rs#L361-L368

https://github.com/astral-sh/uv/blob/4ff057e108a50b2c82dba3c8bf6e38e1c74bc006/crates/uv-resolver/src/error.rs#L619-L626

I think adding the name here would be confusing, since we'd have `Root("my-project") -> Package("my-project")` in the tree?

---

_Converted to draft by @lucab on 2024-09-18 17:21_

---

_Comment by @lucab on 2024-09-19 12:32_

What you say makes plenty of sense in the context of workspaces with packages, and I'm inclined to drop this PR as it may have some side-effect implications that I currently can't fully evaluate.

At the same time, it is notable that this is the only invocation of `pip::operations::resolve()` explicitly without a project.
All the other 5 occurrences do forward a project name, and in virtually all the cases it seems that the source of that value can be traced back to this:
https://github.com/astral-sh/uv/blob/df90cc6f956516a7b53f9cd55df5d7f39a02bd92/crates/uv-requirements/src/specification.rs#L229-L232

---

_Comment by @lucab on 2024-09-20 10:22_

From an out of band discussion with @zanieb: let's keep the current behavior and add a note that it is an explicit choice. 

Closing in favor of https://github.com/astral-sh/uv/pull/7578.

---

_Closed by @lucab on 2024-09-20 10:22_

---

_Branch deleted on 2024-09-20 10:34_

---
