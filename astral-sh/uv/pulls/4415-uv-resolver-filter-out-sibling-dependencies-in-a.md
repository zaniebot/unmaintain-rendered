```yaml
number: 4415
title: "uv-resolver: filter out sibling dependencies in a fork"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/filter-fork-siblings
created_at: 2024-06-19T15:36:21Z
updated_at: 2024-06-20T11:21:46Z
url: https://github.com/astral-sh/uv/pull/4415
synced_at: 2026-01-10T13:54:02Z
```

# uv-resolver: filter out sibling dependencies in a fork

---

_Pull request opened by @BurntSushi on 2024-06-19 15:36_

When a fork is created from a list of dependencies, we were previously
adding all other sibling dependencies to every fork created. But this
isn't actually quite right, since the fork created is always created by
some marker expression. And while it is definitively disjoint from any
directly conflicting dependency specification, it is also possibly
disjoint with other dependencies. For example, as reported in #4414:

```toml
dependencies = [
  "anyio==4.4.0 ; python_version >= '3.12'",
  "anyio==4.3.0 ; python_version < '3.12'",
  "b1 ; python_version >= '3.12'",
  "b2 ; python_version < '3.12'",
]
```

The first two `anyio` requirements are conflicting with non-overlapping
marker expressions, and so a fork is created. Prior to this commit,
*both* `b1` and `b2` would be added to each fork. But of course, `b2` is
impossible in the `anyio==4.4.0` fork because of disjoint marker
expressions.

So in this commit, we specifically filter out any sibling dependencies
that could find their way into a fork that have disjoint markers with
that fork. We are careful to do this both when a new fork is created
from an existing set of dependencies, and when adding new dependencies
to a fork.

Fixes #4414


---

_Comment by @BurntSushi on 2024-06-19 15:38_

I added a packse test here: https://github.com/astral-sh/packse/pull/197

---

_Review requested from @konstin by @BurntSushi on 2024-06-19 15:42_

---

_Review comment by @konstin on `crates/uv/tests/lock_scenarios.rs`:824 on 2024-06-20 11:07_

Should we start censoring the packse version so an upgrade isn't a huge diff each time?

---

_@konstin approved on 2024-06-20 11:07_

---

_Review comment by @BurntSushi on `crates/uv/tests/lock_scenarios.rs`:824 on 2024-06-20 11:21_

cc @zanieb 

I don't think I'm opposed to that. I do try to update the tests in a separate commit so that it's a little easier to review. But I agree there is a fair bit of noise in the diff.

Another possible mitigation here would be a way to update the existing tests without adding any new ones, commit that, and then do an update with the new tests. But I think that'd be a little annoying to do in practice.

---

_@BurntSushi reviewed on 2024-06-20 11:21_

---

_Merged by @BurntSushi on 2024-06-20 11:21_

---

_Closed by @BurntSushi on 2024-06-20 11:21_

---

_Branch deleted on 2024-06-20 11:21_

---
