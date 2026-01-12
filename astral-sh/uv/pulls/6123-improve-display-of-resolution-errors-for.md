```yaml
number: 6123
title: Improve display of resolution errors for workspace member conflicts with optional dependencies
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
  - preview
assignees: []
merged: true
base: main
head: zb/optional-dep-err
created_at: 2024-08-15T17:29:36Z
updated_at: 2024-08-16T01:50:45Z
url: https://github.com/astral-sh/uv/pull/6123
synced_at: 2026-01-12T16:07:13Z
```

# Improve display of resolution errors for workspace member conflicts with optional dependencies

---

_@zanieb_

We have bad error messages for optional (extra) dependencies and development dependencies in workspaces:

1. We weren't showing the full package, so we'd drop `:dev` and `[extra]` by accident
2. We didn't include derived packages, e.g., `member[extra]` in tree processing collapse operation, so we'd include extra clauses like the ones we removed in #6092 

Also

- Reverts https://github.com/astral-sh/uv/pull/6092/commits/f0de4f71f257132388514cbeb96a77215cf90b3b — it turns out it wasn't quite correct and it didn't seem worth using the custom incompatibility anymore.
- Fixes a bug in the display of `package:dev` which was not showing `:dev` for some variants (see 94d8020b589c316f193b8ae4d59430c9b1ba57e7)

---

_Label `error messages` added by @zanieb on 2024-08-15 17:29_

---

_Label `preview` added by @zanieb on 2024-08-15 17:29_

---

_Renamed from "zb/optional dep err" to "Improve display of resolution errors for workspace member conflicts with optional dependencies" by @zanieb on 2024-08-15 17:30_

---

_Converted to draft by @zanieb on 2024-08-15 17:30_

---

_Review comment by @zanieb on `crates/uv/tests/workspace.rs`:1442 on 2024-08-15 18:07_

I cannot figure out why this doesn't say `bar:dev depends on`

---

_@zanieb reviewed on 2024-08-15 18:07_

---

_@zanieb reviewed on 2024-08-15 18:10_

---

_Review comment by @zanieb on `crates/uv/tests/workspace.rs`:1442 on 2024-08-15 18:10_

The tree is wrong

```
      root==0a0.dev0 depends on foo*
        root==0a0.dev0 depends on bar*
          foo==0.1.0 depends on anyio==4.1.0
            bar==0.1.0 depends on anyio==4.2.0
            bar==0.1.0 depends on bar:dev==0.1.0
```

---

_@zanieb reviewed on 2024-08-15 18:11_

---

_Review comment by @zanieb on `crates/uv/tests/workspace.rs`:1442 on 2024-08-15 18:11_

And is wrong before any transformation

```
      root==0a0.dev0 depends on foo*
        root==0a0.dev0 depends on bar*
          no versions of foo<0.1.0 | >0.1.0
            foo==0.1.0 depends on anyio==4.1.0
              bar==0.1.0 depends on anyio==4.2.0
                bar==0.1.0 depends on bar:dev==0.1.0
                no versions of bar<0.1.0 | >0.1.0
```

---

_Marked ready for review by @zanieb on 2024-08-15 18:11_

---

_Comment by @zanieb on 2024-08-15 18:11_

I think we might want to merge this as an incremental improvement. The development dependencies case is weird — it seems like a bug in the resolver itself.

---

_Review requested from @charliermarsh by @zanieb on 2024-08-15 18:35_

---

_@zanieb reviewed on 2024-08-15 20:53_

---

_Review comment by @zanieb on `crates/uv/tests/workspace.rs`:1442 on 2024-08-15 20:53_

This was a display bug https://github.com/astral-sh/uv/pull/6123/files#diff-f8891b530b9722db41a1ef9461f0c2608f639ace189f57ec00e002bf02d7bd00

---

_@charliermarsh approved on 2024-08-16 01:43_

---

_Merged by @zanieb on 2024-08-16 01:50_

---

_Closed by @zanieb on 2024-08-16 01:50_

---

_Branch deleted on 2024-08-16 01:50_

---
