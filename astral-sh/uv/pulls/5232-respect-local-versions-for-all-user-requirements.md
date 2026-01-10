```yaml
number: 5232
title: Respect local versions for all user requirements
type: pull_request
state: merged
author: ibraheemdev
labels:
  - lock
assignees: []
merged: true
base: main
head: ibraheem/fork-locals
created_at: 2024-07-19T20:49:09Z
updated_at: 2024-07-19T21:56:11Z
url: https://github.com/astral-sh/uv/pull/5232
synced_at: 2026-01-10T13:42:52Z
```

# Respect local versions for all user requirements

---

_Pull request opened by @ibraheemdev on 2024-07-19 20:49_

## Summary

This fixes a few bugs introduced by https://github.com/astral-sh/uv/pull/5104. I previously thought we could track conflicting locals the same way we track conflicting URLs in forks, but it turns out that ends up being very tricky. URL forks work because we prioritize directly URL requirements. We can't prioritize locals in the same way without conflicting with the URL prioritization (this may be possible but it's not trivial), so we run into issues where a correct resolution depends on the order in which dependencies are traversed.

Instead, we track local versions across all forks in `Locals`. When applying a local version, we apply all locals with markers that intersect with the current fork. This way we end up applying some local versions without creating a fork. For example, given:
```
// pyproject.toml
dependencies = [
    "torch==2.0.0+cu118 ; platform_machine == 'x86_64'",
]

// requirements.in
torch==2.0.0
.
```

We choose `2.0.0+cu118` in all cases. However, if a disjoint fork is created based on local versions, the resolver will choose the most compatible local when it narrows to a specific fork. Thus we correctly respect local versions when forking:
```
// pyproject.toml
dependencies = [
    "torch==2.0.0+cu118 ; platform_machine == 'x86_64'",
    "torch==2.0.0+cpu ; platform_machine != 'x86_64'"
]

// requirements.in
torch==2.0.0
.
``` 

We should also be able to use a similar strategy for https://github.com/astral-sh/uv/pull/5150.

## Test Plan

This fixes https://github.com/astral-sh/uv/issues/5220 locally for me, as well as a few other bugs that were not reported yet.

---

_Label `lock` added by @ibraheemdev on 2024-07-19 20:49_

---

_Review requested from @charliermarsh by @ibraheemdev on 2024-07-19 20:49_

---

_Review requested from @konstin by @ibraheemdev on 2024-07-19 20:49_

---

_@charliermarsh reviewed on 2024-07-19 21:07_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:2077 on 2024-07-19 21:07_

I guess this will just... fail? Since it has to be the union of multiple different `==` requirements, right?

---

_@charliermarsh approved on 2024-07-19 21:07_

Nice. I find this easier to reason about too.

---

_@ibraheemdev reviewed on 2024-07-19 21:47_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/resolver/mod.rs`:2077 on 2024-07-19 21:47_

This should work fine, we want to allow any compatible local versions before narrowing to a fork.

---

_Merged by @zanieb on 2024-07-19 21:56_

---

_Closed by @zanieb on 2024-07-19 21:56_

---

_Branch deleted on 2024-07-19 21:56_

---
