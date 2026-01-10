```yaml
number: 11091
title: "Don't warn for previously constraint dependencies"
type: pull_request
state: closed
author: konstin
labels:
  - bug
assignees: []
base: main
head: konsti/dont-warn-for-second-req
created_at: 2025-01-30T11:58:32Z
updated_at: 2025-02-03T20:14:31Z
url: https://github.com/astral-sh/uv/pull/11091
synced_at: 2026-01-10T11:10:34Z
```

# Don't warn for previously constraint dependencies

---

_Pull request opened by @konstin on 2025-01-30 11:58_

Fixes the case reported at https://github.com/astral-sh/uv/issues/8870#issuecomment-2622783511

---

_Label `bug` added by @konstin on 2025-01-30 11:58_

---

_@charliermarsh reviewed on 2025-01-30 14:08_

What if a dependency is listed twice in the same optional group, once with a lower bound and once without?

---

_Comment by @konstin on 2025-01-30 14:20_

You mean like this? They are already not emitting warnings. (Which looks like a false negative? They are not showing warnings even without the bounds.)

```
[project]
name = "foo"
version = "0.1.0"
requires-python = ">=3.10"
dependencies = []

[project.optional-dependencies]
case_a = [
  "anyio",
  "anyio>=4,<5",
]
case_b1 = [
  "numpy",
]
case_b2 = [
  "numpy>=2",
]
```

---

_@charliermarsh reviewed on 2025-01-30 14:22_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/metadata/lowering.rs`:49 on 2025-01-30 14:22_

We should be using `FxHashSet` by default.

---

_@charliermarsh approved on 2025-01-30 14:23_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/metadata/lowering.rs`:145 on 2025-01-30 14:25_

So, should we remove this? I think it's redundant now? (Also, isn't this wrong? It seems to only look at `None`, and not the lower bound?)

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/metadata/lowering.rs`:663 on 2025-01-30 14:26_

I'm actually slightly confused now, hmm... You have this `has_lower_bound` check in `get_constrained_packages`, but here we're not looking at the specifier... We're just warning if there's no constraint.

---

_@charliermarsh reviewed on 2025-01-30 14:26_

---

_Comment by @charliermarsh on 2025-01-30 14:28_

Becoming slightly confused about the difference between how `constrained_packages` is populated and when we choose to show these warnings (i.e., checking `has_lower_bound` vs. checking if the specifiers are `None`).

---

_Closed by @konstin on 2025-02-03 20:14_

---
