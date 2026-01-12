```yaml
number: 3854
title: "Move shadow tracking into `Scope` directly"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/rebounds
created_at: 2023-04-02T22:06:40Z
updated_at: 2023-04-03T19:33:45Z
url: https://github.com/astral-sh/ruff/pull/3854
synced_at: 2026-01-12T15:55:13Z
```

# Move shadow tracking into `Scope` directly

---

_@charliermarsh_

_No description provided._

---

_Comment by @github-actions[bot] on 2023-04-02 22:22_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.4±0.09ms     2.8 MB/sec    1.00     14.4±0.08ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    454.8±1.28µs     6.5 MB/sec    1.00    454.2±1.29µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.03ms     4.2 MB/sec    1.01      6.1±0.18ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.13ms     5.6 MB/sec    1.01      7.3±0.14ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1634.8±38.30µs    10.2 MB/sec    1.00   1637.0±4.40µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    179.7±0.49µs    16.4 MB/sec    1.00    180.1±0.45µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.6 MB/sec    1.00      3.4±0.02ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.05     20.6±0.98ms  2025.2 KB/sec    1.00     19.6±0.53ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.22ms     3.2 MB/sec    1.00      5.1±0.25ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   606.8±27.41µs     4.9 MB/sec    1.00   605.2±33.55µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.3±0.30ms     3.1 MB/sec    1.03      8.6±0.38ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.03     10.6±0.32ms     3.8 MB/sec    1.00     10.3±0.43ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.3±0.08ms     7.3 MB/sec    1.00      2.2±0.08ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   260.7±13.96µs    11.3 MB/sec    1.03   268.7±28.51µs    11.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.9±0.19ms     5.2 MB/sec    1.02      5.0±0.28ms     5.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/scope.rs`:20 on 2023-04-03 07:28_

I wonder if using a flag `Vec` would be faster because most variables in a scope don't get shadowed. Scanning a small flat vec can be faster than searching  in a nested hash map (particularly if all entries fit into a single cache line, one entry here requires 12 bytes -> 5 entries fit into a cache line). 

Another solution could be to change `bindings` to `FxHashMap<&'a str, SmallVec<[BindingId; 2]>` . This has the benefit that we avoid allocating for almost all bindings (you have to double check if I did the math correctly but I think that `SmallVec<[BindingId;2]>` has the same size as `Vec<BindingId>`) and don't need another data structure for tracking. 

---

_@MichaReiser approved on 2023-04-03 07:34_

---

_@charliermarsh reviewed on 2023-04-03 15:30_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/scope.rs`:20 on 2023-04-03 15:30_

Yeah this is why I also considered at one point doing:

```rust
bindings: FxHashMap<&'a str, (BindingId, Vec<BindingId>)>
```

Or similar. Since we're guaranteed to have at least one binding, and in most cases we don't need to use the extra `Vec` at all.

When you suggest using a `Vec` here for `shadowed_bindings`, do you mean something like `Vec<(&'a str, BindingId)>`?


---

_@MichaReiser reviewed on 2023-04-03 18:13_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/scope.rs`:20 on 2023-04-03 18:13_

> When you suggest using a Vec here for shadowed_bindings, do you mean something like Vec<(&'a str, BindingId)>?

Exactly (but without anonymous types :D)

---

_@charliermarsh reviewed on 2023-04-03 19:33_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/scope.rs`:20 on 2023-04-03 19:33_

I'm going to leave as-is because we barely use these right now (only in a single rule) so it's hard for me to assess performance patterns in the real-world.

---

_Merged by @charliermarsh on 2023-04-03 19:33_

---

_Closed by @charliermarsh on 2023-04-03 19:33_

---

_Branch deleted on 2023-04-03 19:33_

---
