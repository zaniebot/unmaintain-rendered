```yaml
number: 7701
title: Allow unused mut in graph resolution conflicts
type: pull_request
state: merged
author: lfn3
labels: []
assignees: []
merged: true
base: main
head: allow-mut-graph-res-conflicting-distributions
created_at: 2024-09-26T09:11:32Z
updated_at: 2024-09-26T09:17:43Z
url: https://github.com/astral-sh/uv/pull/7701
synced_at: 2026-01-10T12:53:53Z
```

# Allow unused mut in graph resolution conflicts

---

_Pull request opened by @lfn3 on 2024-09-26 09:11_

## Summary

When running:
`cargo install --git https://github.com/astral-sh/uv uv` I got the following warning:
```
warning: variable does not need to be mutable
   --> crates/uv-resolver/src/resolution/graph.rs:249:13
    |
249 |         let mut conflicting = graph.find_conflicting_distributions();
    |             ----^^^^^^^^^^^
    |             |
    |             help: remove this `mut`
    |
    = note: `#[warn(unused_mut)]` on by default
```

This change just allows the mut, since it's used in a #[cfg(debug_assertions)] below

## Test Plan

Given the change should not make it into the compiled artifact, don't think much in the way of testing is warranted, but please 
correct me if I'm wrong.

---

_@konstin approved on 2024-09-26 09:12_

Thanks!

---

_Merged by @konstin on 2024-09-26 09:17_

---

_Closed by @konstin on 2024-09-26 09:17_

---
