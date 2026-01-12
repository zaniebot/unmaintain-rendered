```yaml
number: 4138
title: "Remove `ScopeStack` in favor of child-parent `ScopeId` pointers"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/scopes
created_at: 2023-04-28T03:51:46Z
updated_at: 2023-04-29T23:11:25Z
url: https://github.com/astral-sh/ruff/pull/4138
synced_at: 2026-01-12T15:55:14Z
```

# Remove `ScopeStack` in favor of child-parent `ScopeId` pointers

---

_@charliermarsh_

## Summary

Currently, we maintain a stack of `ScopeId` elements to track the current stack of scopes (module, function, etc.). This makes iteration and scope traversal very easy, but a common operation we perform is "Save the current scope, so we can restore it later" -- and with our current design, that operation becomes unnecessarily expensive, as we need to clone the entire vector of `ScopeId` elements in order to save our position in the stack.

This PR instead migrates to a model in which we store the parent `ScopeId` on every scope, which in turn allows us to merely store the current `ScopeId` when saving and restoring the current scope.

We could introduce a crate like [`indextree`](https://docs.rs/indextree) to help with this, and it may make sense to do so in the future, but for now, I've just implemented this tracking manually, since it's very straightforward.

## Test Plan

This improves benchmark performance by a couple of percentage points:

```
❯ cargo benchmark --baseline=main
    Finished bench [optimized] target(s) in 0.14s
     Running benches/linter.rs (target/release/deps/linter-200959ba3da63cb3)
linter/default-rules/numpy/globals.py
                        time:   [72.278 µs 72.403 µs 72.539 µs]
                        thrpt:  [40.677 MiB/s 40.753 MiB/s 40.824 MiB/s]
                 change:
                        time:   [-1.1862% -0.8772% -0.5622%] (p = 0.00 < 0.05)
                        thrpt:  [+0.5654% +0.8850% +1.2005%]
                        Change within noise threshold.
Found 11 outliers among 100 measurements (11.00%)
  10 (10.00%) high mild
  1 (1.00%) high severe
linter/default-rules/pydantic/types.py
                        time:   [1.4848 ms 1.4873 ms 1.4896 ms]
                        thrpt:  [17.120 MiB/s 17.147 MiB/s 17.176 MiB/s]
                 change:
                        time:   [-1.8008% -1.3902% -0.9916%] (p = 0.00 < 0.05)
                        thrpt:  [+1.0015% +1.4098% +1.8338%]
                        Change within noise threshold.
Found 1 outliers among 100 measurements (1.00%)
  1 (1.00%) high mild
linter/default-rules/numpy/ctypeslib.py
                        time:   [684.87 µs 685.87 µs 686.82 µs]
                        thrpt:  [24.244 MiB/s 24.277 MiB/s 24.313 MiB/s]
                 change:
                        time:   [-4.2976% -3.6049% -2.9363%] (p = 0.00 < 0.05)
                        thrpt:  [+3.0251% +3.7397% +4.4906%]
                        Performance has improved.
Found 1 outliers among 100 measurements (1.00%)
  1 (1.00%) high mild
linter/default-rules/large/dataset.py
                        time:   [3.4197 ms 3.4205 ms 3.4213 ms]
                        thrpt:  [11.891 MiB/s 11.894 MiB/s 11.896 MiB/s]
                 change:
                        time:   [-1.8290% -1.5224% -1.2507%] (p = 0.00 < 0.05)
                        thrpt:  [+1.2666% +1.5460% +1.8631%]
                        Performance has improved.
Found 12 outliers among 100 measurements (12.00%)
  6 (6.00%) low severe
  4 (4.00%) low mild
  1 (1.00%) high mild
  1 (1.00%) high severe

linter/all-rules/numpy/globals.py
                        time:   [144.15 µs 144.35 µs 144.56 µs]
                        thrpt:  [20.411 MiB/s 20.441 MiB/s 20.469 MiB/s]
                 change:
                        time:   [-0.2085% +0.1292% +0.5449%] (p = 0.52 > 0.05)
                        thrpt:  [-0.5420% -0.1291% +0.2090%]
                        No change in performance detected.
Found 8 outliers among 100 measurements (8.00%)
  7 (7.00%) high mild
  1 (1.00%) high severe
linter/all-rules/pydantic/types.py
                        time:   [2.9117 ms 2.9158 ms 2.9199 ms]
                        thrpt:  [8.7343 MiB/s 8.7465 MiB/s 8.7589 MiB/s]
                 change:
                        time:   [-1.2509% -0.7777% -0.3998%] (p = 0.00 < 0.05)
                        thrpt:  [+0.4014% +0.7838% +1.2668%]
                        Change within noise threshold.
linter/all-rules/numpy/ctypeslib.py
                        time:   [1.6440 ms 1.6464 ms 1.6488 ms]
                        thrpt:  [10.099 MiB/s 10.114 MiB/s 10.128 MiB/s]
                 change:
                        time:   [-1.0992% -0.8788% -0.6667%] (p = 0.00 < 0.05)
                        thrpt:  [+0.6712% +0.8866% +1.1114%]
                        Change within noise threshold.
Found 1 outliers among 100 measurements (1.00%)
  1 (1.00%) high mild
linter/all-rules/large/dataset.py
                        time:   [6.9923 ms 6.9971 ms 7.0015 ms]
                        thrpt:  [5.8106 MiB/s 5.8143 MiB/s 5.8182 MiB/s]
                 change:
                        time:   [-1.6946% -1.4648% -1.2491%] (p = 0.00 < 0.05)
                        thrpt:  [+1.2649% +1.4866% +1.7239%]
                        Performance has improved.
Found 7 outliers among 100 measurements (7.00%)
  5 (5.00%) low mild
  2 (2.00%) high severe
```

I also experimented with a design in which we stored parallel vectors on `Scopes`: a vector of `Scope` structs, and a parallel vector of parent pointers (`ScopeId`). While I expected this to perform _better_ (SoA vs. AoS), it actually performed worse in practice 🤔 


---

_Comment by @charliermarsh on 2023-04-28 03:59_

(A couple things I want to clean up here but if anything seems way off base, feel free to flag it @MichaReiser.)

---

_Comment by @github-actions[bot] on 2023-04-28 04:09_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.0±0.06ms     2.9 MB/sec    1.00     14.0±0.05ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    418.9±1.36µs     7.0 MB/sec    1.01    421.9±0.57µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.02ms     4.4 MB/sec    1.00      5.8±0.03ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.01ms     5.8 MB/sec    1.00      7.0±0.01ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1492.1±1.26µs    11.2 MB/sec    1.00   1489.1±2.34µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    167.8±0.24µs    17.6 MB/sec    1.00    167.8±1.77µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.00ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.6±0.01ms     7.3 MB/sec    1.08      6.0±0.01ms     6.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1073.3±0.68µs    15.5 MB/sec    1.07   1153.4±0.47µs    14.4 MB/sec
parser/numpy/globals.py                    1.00    108.6±0.22µs    27.2 MB/sec    1.06    115.6±0.17µs    25.5 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    10.9 MB/sec    1.08      2.5±0.00ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.5±0.19ms     2.5 MB/sec    1.01     16.6±0.22ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.04ms     4.0 MB/sec    1.01      4.2±0.06ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    489.4±6.01µs     6.0 MB/sec    1.01   494.5±10.00µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.16ms     3.7 MB/sec    1.00      7.0±0.13ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.08ms     4.9 MB/sec    1.00      8.4±0.08ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1776.6±28.24µs     9.4 MB/sec    1.02  1804.9±21.99µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    200.6±3.43µs    14.7 MB/sec    1.02    205.2±6.35µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.04ms     6.8 MB/sec    1.01      3.8±0.06ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.7±0.08ms     6.0 MB/sec    1.00      6.7±0.06ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1277.2±12.60µs    13.0 MB/sec    1.01  1292.0±27.90µs    12.9 MB/sec
parser/numpy/globals.py                    1.00    131.0±2.21µs    22.5 MB/sec    1.01    131.9±2.15µs    22.4 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.04ms     9.0 MB/sec    1.00      2.9±0.03ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @MichaReiser by @charliermarsh on 2023-04-28 21:12_

---

_@charliermarsh reviewed on 2023-04-28 21:14_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/context.rs`:394 on 2023-04-28 21:14_

I removed this method since `scope_id` is still `pub`, and it's confusing (IMO) to have a getter for a public field.

It's hard to use this method everywhere (same with some of the utilities, like `scopes`) because of the borrow checker semantics. Often, we borrow (e.g.) `self.ctx.scope_id`, then mutate `self.ctx.bindings` or something similar. The borrow checker is fine with that. But if we use `self.ctx.scope_id()`, we can't mutate other parts of `self.ctx`, since that's considered an immutable borrow of `self.ctx`.

---

_@charliermarsh reviewed on 2023-04-28 21:14_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/scope.rs`:215 on 2023-04-28 21:14_

A bit odd to have two methods here, but sort of convenient...

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyflakes/rules/undefined_local.rs`:22 on 2023-04-28 21:15_

Refactored this a bit, it should also be more performant (previously, we created a `Vec` of `Scope` to pass in every time).

---

_@charliermarsh reviewed on 2023-04-28 21:15_

---

_@charliermarsh reviewed on 2023-04-28 21:15_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:4176 on 2023-04-28 21:15_

(Renamed a variety of `_index` usages to `_id`.)

---

_Marked ready for review by @charliermarsh on 2023-04-28 21:15_

---

_@MichaReiser reviewed on 2023-04-29 15:34_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/context.rs`:394 on 2023-04-29 15:34_

Hmm, `scope_id` shouldn't borrow because it's copied. I do see value in migrating towards getters to avoid our very loose APIs and long getter chains to context-specific data structures. 

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:282 on 2023-04-29 15:36_

I would expect that `scopes.ancestors` returns an `Iterator<Item=&Scope>` and not an iterator over the scope ids (`ancestor_ids`?). Returning a `Scope` iterator would hide some of the `id` to scope mapping complexity.

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:1699 on 2023-04-29 15:37_

Is it still necessary to clone `parents` or can we even remove `parents`?

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:4848 on 2023-04-29 15:39_

Nice

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pyflakes/rules/undefined_local.rs`:23 on 2023-04-29 15:41_

Nit: Create a `ctx.scope()` and `ctx.scope_mut()` methods that returns the current scope

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/scope.rs`:215 on 2023-04-29 15:47_

Nit: I would flip the two so that `ancestors` returns an iterator of `Scope`s and `ancestor_ids` returns the ids only. 

The `ids` operator may not be required because the `id` can be accessed by using `Scope.id()`. 

---

_@MichaReiser approved on 2023-04-29 15:50_

Nice! 

I have one question and one improvement suggestion:

* What's currently preventing us from removing `ctx.parents`? 
* I think it should now be possible to remove `Scope.id` by:
	* Internally storing a `ScopeData` that matches today's `Scope` without the `id`
	* Create a `Scope` struct that references a `ScopeData` and stores the `id` with the scope
	* The iterator constructs `Scope`s and uses the implicit `index` of the `Scope` as the `ScopeId`. 	

---

_@charliermarsh reviewed on 2023-04-29 18:25_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/context.rs`:394 on 2023-04-29 18:25_

Oh, hmm, perhaps that does work here, I have to check. (It doesn't work in general for references, but you're right that I'd expect it to work with copies.)

I don't disagree, but if we add / retain this, I think we should try to remove `pub` from the `scope_id` attribute. Otherwise, it's unclear which access pattern to use (and editor completion is confusing too).

---

_@charliermarsh reviewed on 2023-04-29 22:03_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:1699 on 2023-04-29 22:03_

I think we can remove `parents` soon, but it requires a similar approach. We do need to set the list of parents when we "restore" state, but we should be able to use IDs with parent pointers, just like with scopes.

---

_Comment by @charliermarsh on 2023-04-29 22:13_

I was able to remove `Scope.id`. We now use `ancestor_ids` in one place, so I'm tempted to just leave it as-is and not expose a `Scope.id()` method at all.

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:1699 on 2023-04-29 22:17_

We may also want to consider using `indextree` for statements, to enable us to traverse up and get symbolic representations?

---

_@charliermarsh reviewed on 2023-04-29 22:17_

---

_Comment by @charliermarsh on 2023-04-29 22:22_

To clarify: separate `ScopeData` and `Scope` structs may be unnecessary, since we only need the ID at one call-site, so perhaps we just skip that abstraction for now?

---

_Merged by @charliermarsh on 2023-04-29 22:23_

---

_Closed by @charliermarsh on 2023-04-29 22:23_

---

_Branch deleted on 2023-04-29 22:23_

---

_Comment by @charliermarsh on 2023-04-29 22:24_

Biasing towards merging but always happy to handle follow-ups in new PRs :)

---

_@charliermarsh reviewed on 2023-04-29 23:11_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:1699 on 2023-04-29 23:11_

(Ignore the `indextree` reference here, I think we can make improvements to parent tracking but that crate might end up being irrelevant.)

---
