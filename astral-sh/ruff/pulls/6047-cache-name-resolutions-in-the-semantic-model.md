```yaml
number: 6047
title: Cache name resolutions in the semantic model
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
  - performance
assignees: []
merged: true
base: main
head: charlie/resolve-call-path
created_at: 2023-07-24T22:34:52Z
updated_at: 2023-07-27T17:02:49Z
url: https://github.com/astral-sh/ruff/pull/6047
synced_at: 2026-01-12T03:30:22Z
```

# Cache name resolutions in the semantic model

---

_Pull request opened by @charliermarsh on 2023-07-24 22:34_

## Summary

This PR stores the mapping from `ExprName` node to resolved `BindingId`, which lets us skip scope lookups in `resolve_call_path`. It's enabled by #6045, since that PR ensures that when we analyze a node (and thus call `resolve_call_path`), we'll have already visited its `ExprName` elements.

In more detail: imagine that we're traversing over `foo.bar()`. When we read `foo`, it will be an `ExprName`, which we'll then resolve to a binding via `handle_node_load`. With this change, we then store that binding in a map. Later, if we call `collect_call_path` on `foo.bar`, we'll identify `foo` (the "head" of the attribute) and grab the resolved binding in that map. _Almost_ all names are now resolved in advance, though it's not a strict requirement, and some rules break that pattern (e.g., if we're analyzing arguments, and they need to inspect their annotations, which are visited in a deferred manner).

This improves performance by 4-6% on the all-rules benchmark. It looks like it hurts performance (1-2% drop) in the default-rules benchmark, presumedly because those rules don't call `resolve_call_path` nearly as much, and so we're paying for these extra writes.

Here's the benchmark data:

```
linter/default-rules/numpy/globals.py
                        time:   [67.270 µs 67.380 µs 67.489 µs]
                        thrpt:  [43.720 MiB/s 43.792 MiB/s 43.863 MiB/s]
                 change:
                        time:   [+0.4747% +0.7752% +1.0626%] (p = 0.00 < 0.05)
                        thrpt:  [-1.0514% -0.7693% -0.4724%]
                        Change within noise threshold.
Found 1 outliers among 100 measurements (1.00%)
  1 (1.00%) high severe
linter/default-rules/pydantic/types.py
                        time:   [1.4067 ms 1.4105 ms 1.4146 ms]
                        thrpt:  [18.028 MiB/s 18.081 MiB/s 18.129 MiB/s]
                 change:
                        time:   [+1.3152% +1.6953% +2.0414%] (p = 0.00 < 0.05)
                        thrpt:  [-2.0006% -1.6671% -1.2981%]
                        Performance has regressed.
linter/default-rules/numpy/ctypeslib.py
                        time:   [637.67 µs 638.96 µs 640.28 µs]
                        thrpt:  [26.006 MiB/s 26.060 MiB/s 26.113 MiB/s]
                 change:
                        time:   [+1.5859% +1.8109% +2.0353%] (p = 0.00 < 0.05)
                        thrpt:  [-1.9947% -1.7787% -1.5611%]
                        Performance has regressed.
linter/default-rules/large/dataset.py
                        time:   [3.2289 ms 3.2336 ms 3.2383 ms]
                        thrpt:  [12.563 MiB/s 12.581 MiB/s 12.599 MiB/s]
                 change:
                        time:   [+0.8029% +0.9898% +1.1740%] (p = 0.00 < 0.05)
                        thrpt:  [-1.1604% -0.9801% -0.7965%]
                        Change within noise threshold.

linter/all-rules/numpy/globals.py
                        time:   [134.05 µs 134.15 µs 134.26 µs]
                        thrpt:  [21.977 MiB/s 21.995 MiB/s 22.012 MiB/s]
                 change:
                        time:   [-4.4571% -4.1175% -3.8268%] (p = 0.00 < 0.05)
                        thrpt:  [+3.9791% +4.2943% +4.6651%]
                        Performance has improved.
Found 8 outliers among 100 measurements (8.00%)
  2 (2.00%) low mild
  3 (3.00%) high mild
  3 (3.00%) high severe
linter/all-rules/pydantic/types.py
                        time:   [2.5627 ms 2.5669 ms 2.5720 ms]
                        thrpt:  [9.9158 MiB/s 9.9354 MiB/s 9.9516 MiB/s]
                 change:
                        time:   [-5.8304% -5.6374% -5.4452%] (p = 0.00 < 0.05)
                        thrpt:  [+5.7587% +5.9742% +6.1914%]
                        Performance has improved.
Found 7 outliers among 100 measurements (7.00%)
  6 (6.00%) high mild
  1 (1.00%) high severe
linter/all-rules/numpy/ctypeslib.py
                        time:   [1.3949 ms 1.3956 ms 1.3964 ms]
                        thrpt:  [11.925 MiB/s 11.931 MiB/s 11.937 MiB/s]
                 change:
                        time:   [-6.2496% -6.0856% -5.9293%] (p = 0.00 < 0.05)
                        thrpt:  [+6.3030% +6.4799% +6.6662%]
                        Performance has improved.
Found 7 outliers among 100 measurements (7.00%)
  3 (3.00%) high mild
  4 (4.00%) high severe
linter/all-rules/large/dataset.py
                        time:   [5.5951 ms 5.6019 ms 5.6093 ms]
                        thrpt:  [7.2527 MiB/s 7.2623 MiB/s 7.2711 MiB/s]
                 change:
                        time:   [-5.1781% -4.9783% -4.8070%] (p = 0.00 < 0.05)
                        thrpt:  [+5.0497% +5.2391% +5.4608%]
                        Performance has improved.
```

Still playing with this (the concepts need better names, documentation, etc.), but opening up for feedback.

---

_Comment by @github-actions[bot] on 2023-07-24 22:44_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-25 00:08_

---

_Comment by @charliermarsh on 2023-07-25 01:40_

After this, it seems like pretty much all of `resolve_call_path` is spent in the `extend_from_slice` calls... I'm trying to figure out how to avoid those allocations. We're always concatenating two call paths to create a single call path.

---

_Comment by @charliermarsh on 2023-07-25 01:43_

For context, `resolve_call_path` is 9.3% of execution time for Airflow (all-rules), `extend_from_slice` is 4.4%. So "pretty much all" is an exaggeration, but it's a lot.

---

_Comment by @charliermarsh on 2023-07-25 01:43_

Oh, maybe that's time spent in `from_unqualified_name` actually. I might be misreading Instruments.

---

_Marked ready for review by @charliermarsh on 2023-07-26 01:22_

---

_Comment by @charliermarsh on 2023-07-26 01:22_

Ready for review.

---

_Label `internal` added by @charliermarsh on 2023-07-26 01:24_

---

_Label `performance` added by @charliermarsh on 2023-07-26 01:24_

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:1550 on 2023-07-26 06:58_

Could this be handled inside `resolve_load`, since the caching is also handled in the semantic model?

---

_@MichaReiser approved on 2023-07-26 07:01_

Looks reasonable to me. What's the impact on memory consumption? 

Is there any locality involved when working with call paths? E.g. can we assume that rules mainly query call paths of the currently visited node? If so, it may be an opportunity to only store a very few nodes (in a `Vec`?) rather than all ever seen call paths. 

---

_@konstin approved on 2023-07-26 09:50_

---

_@charliermarsh reviewed on 2023-07-26 20:11_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:1550 on 2023-07-26 20:11_

Yeah, it can.

---

_Comment by @charliermarsh on 2023-07-27 13:23_

In the hyperfine benchmarks, this improves Airflow's all-rules performance by ~3.68%, with no degradation on the default ruleset.

---

_Merged by @charliermarsh on 2023-07-27 17:01_

---

_Closed by @charliermarsh on 2023-07-27 17:01_

---

_Branch deleted on 2023-07-27 17:01_

---

_Comment by @charliermarsh on 2023-07-27 17:02_

I can't figure out a reliable way to benchmark the memory consumption, because even repeated runs on main are giving me some variation in total allocations. It shouldn't be prohibitively expensive though, since we're just saving an extra u32 for every "load" Name node...

---
