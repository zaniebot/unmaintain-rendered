```yaml
number: 6010
title: "Avoid overwriting dependencies with different markers in `uv add`"
type: pull_request
state: merged
author: blueraft
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: avoid-marker-collisions
created_at: 2024-08-11T17:25:31Z
updated_at: 2024-08-16T20:46:47Z
url: https://github.com/astral-sh/uv/pull/6010
synced_at: 2026-01-12T16:07:09Z
```

# Avoid overwriting dependencies with different markers in `uv add`

---

_@blueraft_

## Summary

Splitting out https://github.com/astral-sh/uv/pull/6005

## Test Plan

`cargo test`

---

_@blueraft reviewed on 2024-08-11 17:30_

---

_Review comment by @blueraft on `crates/uv/src/commands/project/remove.rs`:98 on 2024-08-11 17:30_

I don't know if we can avoid doing this conversion here. All of the `remove_dependency` functions rely on the `find_dependencies` function which requires the `Requirement` type. `\cc @zanieb 

---

_@zanieb reviewed on 2024-08-11 20:46_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/remove.rs`:98 on 2024-08-11 20:46_

From a quick scan of the changes, it seems like there are a couple alternative options:

1. Perform marker filtering on the result of `find_dependencies` when relevant or...
2. Update `find_dependencies` to take `Option<Marker>` in addition to `PackageName`



---

_Comment by @zanieb on 2024-08-11 20:47_

Not sure if this is covered, but we'll probably want `uv remove foo` to remove all dependencies on `foo` regardless of the markers?

---

_Comment by @ibraheemdev on 2024-08-11 21:24_

Haven't read the PR in detail yet, but if there is only a single instance of the package name can we update it regardless of the markers (without overwriting them if unspecified)? For example, `uv add requests; os_name == 'Linux'` followed by `uv add requests==2.0.0`: should the original dependency be updated or should we add a new entry?

---

_Comment by @zanieb on 2024-08-12 12:36_

>  For example, uv add requests; os_name == 'Linux' followed by uv add requests==2.0.0: should the original dependency be updated or should we add a new entry?

Hard choice here — but if we update the existing entry here then there's no way to add a dependency _without_ a marker so I think we need to add a new entry.

---

_@blueraft reviewed on 2024-08-12 12:36_

---

_Review comment by @blueraft on `crates/uv/src/commands/project/remove.rs`:98 on 2024-08-12 12:36_

I've gone with the second option

---

_Comment by @blueraft on 2024-08-12 12:45_

> Not sure if this is covered, but we'll probably want uv remove foo to remove all dependencies on foo regardless of the markers?

It should be covered now. 

> For example, uv add requests; os_name == 'Linux' followed by uv add requests==2.0.0: should the original dependency be updated or should we add a new entry?

I agree with Zanie. If we were to update, we would also have to consider version comparisons before updating. `uv add requests==2.30; os_name == 'Linux'` followed by `uv add requests>=2.0.0` should create an additional entry.

---

_@zanieb reviewed on 2024-08-12 12:57_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/remove.rs`:226 on 2024-08-12 12:57_

Can you revert the variable changes here where it's a `PackageName` again?

---

_@blueraft reviewed on 2024-08-12 13:10_

---

_Review comment by @blueraft on `crates/uv/src/commands/project/remove.rs`:226 on 2024-08-12 13:10_

Done

---

_Comment by @blueraft on 2024-08-12 13:28_

Hm something went wrong with Github Actions there, why is it building the docker image? 🤔

---

_Label `cli` added by @charliermarsh on 2024-08-16 17:26_

---

_Label `preview` added by @charliermarsh on 2024-08-16 17:26_

---

_@zanieb approved on 2024-08-16 20:40_

Thanks!

---

_Renamed from "Avoid overwriting different markers" to "Avoid overwriting dependencies with different markers in `uv add`" by @zanieb on 2024-08-16 20:43_

---

_Merged by @zanieb on 2024-08-16 20:46_

---

_Closed by @zanieb on 2024-08-16 20:46_

---
