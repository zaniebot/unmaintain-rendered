```yaml
number: 3853
title: "Use references for `Export` binding type"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/all
created_at: 2023-04-02T21:17:54Z
updated_at: 2023-04-03T15:47:59Z
url: https://github.com/astral-sh/ruff/pull/3853
synced_at: 2026-01-12T15:55:13Z
```

# Use references for `Export` binding type

---

_@charliermarsh_

## Summary

Non-behavioral refactor to use references for `Export` bindings. For context, `Export` bindings are those created via assignment to Python's special `__all__` identifier:

```py
__all__ = ["foo", "bar"]
```

This also removes a dependency in `all.rs` on `Context`.


---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:4459 on 2023-04-02 21:19_

This used to happen within `extract_all_names`, but it makes more sense to keep `extract_all_names` distinct from this existing-binding lookup.

---

_@charliermarsh reviewed on 2023-04-02 21:19_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-04-02 21:19_

---

_@charliermarsh reviewed on 2023-04-02 21:20_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/all.rs`:15 on 2023-04-02 21:20_

Is there any downside to using a closures here (as opposed to passing in `Context`)? I'd like to do this in a few other places to remove dependencies on `Context`...

---

_Comment by @github-actions[bot] on 2023-04-02 21:31_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.1±0.05ms     2.9 MB/sec    1.01     14.2±0.03ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.01      3.6±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    453.6±1.26µs     6.5 MB/sec    1.01    459.3±1.04µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.02ms     4.3 MB/sec    1.01      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.02ms     5.6 MB/sec    1.01      7.3±0.01ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1627.9±4.96µs    10.2 MB/sec    1.02   1658.1±3.55µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.5±0.54µs    16.3 MB/sec    1.00    182.3±0.47µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.7 MB/sec    1.02      3.4±0.01ms     7.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     20.4±1.01ms  2047.0 KB/sec    1.03     21.0±1.15ms  1979.4 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      5.4±0.29ms     3.1 MB/sec    1.00      5.2±0.22ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   623.7±19.99µs     4.7 MB/sec    1.01   628.1±34.71µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.7±0.38ms     2.9 MB/sec    1.00      8.6±0.29ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00     10.3±0.33ms     3.9 MB/sec    1.01     10.5±0.49ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.3±0.10ms     7.4 MB/sec    1.02      2.3±0.10ms     7.2 MB/sec
linter/default-rules/numpy/globals.py      1.01   276.1±13.48µs    10.7 MB/sec    1.00   273.3±18.96µs    10.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.18ms     5.5 MB/sec    1.04      4.8±0.37ms     5.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-04-03 07:21_

---

_@MichaReiser reviewed on 2023-04-03 07:24_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/all.rs`:15 on 2023-04-03 07:24_

Not necessarily. It can make it more difficult to call the function because I have to figure out what to pass for `is_builtin` whereas I probably know from where I can get `context`. The way I feel about it is that it's similar to using unnamed types. Using a function falls back to unnamed types because we don't have better abstractions. This can be a feasible choice but it convey's less information, making it more difficult to understand. That's why I think this is a reasonable short term solution but we should strive to find the right abstractions (e.g. how about defining a `SemanticModel` and pass it here instead?)

Can we add some documentation for `is_builtin` to document its semantics? 

---

_Merged by @charliermarsh on 2023-04-03 15:26_

---

_Closed by @charliermarsh on 2023-04-03 15:26_

---

_Branch deleted on 2023-04-03 15:26_

---
