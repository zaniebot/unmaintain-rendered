```yaml
number: 6347
title: "Rename semantic model methods to use `current_*` prefix"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/model-names
created_at: 2023-08-04T16:41:34Z
updated_at: 2023-08-07T15:05:31Z
url: https://github.com/astral-sh/ruff/pull/6347
synced_at: 2026-01-12T02:52:04Z
```

# Rename semantic model methods to use `current_*` prefix

---

_Pull request opened by @charliermarsh on 2023-08-04 16:41_

## Summary

This PR attempts to draw a clearer divide between "methods that take (e.g.) an expression or statement as input" and "methods that rely on the _current_ expression or statement" in the semantic model, by renaming methods like `stmt()` to `current_statement()`.

This had led to confusion in the past. For example, prior to this PR, we had `scope()` (which returns the current scope), and `parent_scope`, which returns the parent _of a scope that's passed in_. Now, the API is clearer: `current_scope` returns the current scope, and `parent_scope` takes a scope as argument and returns its parent.

Per above, I also changed `stmt` to `statement` and `expr` to `expression`.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-04 16:41_

---

_Review requested from @zanieb by @charliermarsh on 2023-08-04 16:41_

---

_Label `internal` added by @charliermarsh on 2023-08-04 16:41_

---

_@zanieb approved on 2023-08-04 16:44_

Much better

---

_Comment by @github-actions[bot] on 2023-08-04 16:54_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.6±0.20ms     4.7 MB/sec    1.04      8.9±0.35ms     4.6 MB/sec
formatter/numpy/ctypeslib.py               1.00  1693.9±43.71µs     9.8 MB/sec    1.01  1711.7±43.08µs     9.7 MB/sec
formatter/numpy/globals.py                 1.00    189.8±6.86µs    15.5 MB/sec    1.04    197.8±8.68µs    14.9 MB/sec
formatter/pydantic/types.py                1.00      3.7±0.20ms     6.9 MB/sec    1.01      3.8±0.12ms     6.8 MB/sec
linter/all-rules/large/dataset.py          1.02     11.1±0.37ms     3.6 MB/sec    1.00     11.0±0.24ms     3.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.0±0.13ms     5.6 MB/sec    1.00      2.9±0.06ms     5.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   413.8±12.29µs     7.1 MB/sec    1.00   412.1±10.23µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.0±0.11ms     5.1 MB/sec    1.00      4.9±0.12ms     5.2 MB/sec
linter/default-rules/large/dataset.py      1.00      5.7±0.17ms     7.1 MB/sec    1.00      5.7±0.13ms     7.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1239.7±62.93µs    13.4 MB/sec    1.00  1232.9±69.83µs    13.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    136.2±4.22µs    21.7 MB/sec    1.01    137.1±3.90µs    21.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.07ms    10.1 MB/sec    1.01      2.6±0.07ms     9.9 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.02     12.5±0.47ms     3.2 MB/sec     1.00     12.3±0.51ms     3.3 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.4±0.11ms     7.0 MB/sec     1.00      2.3±0.14ms     7.1 MB/sec
formatter/numpy/globals.py                 1.01   266.3±16.50µs    11.1 MB/sec     1.00   263.9±17.75µs    11.2 MB/sec
formatter/pydantic/types.py                1.02      5.2±0.22ms     4.9 MB/sec     1.00      5.1±0.23ms     5.0 MB/sec
linter/all-rules/large/dataset.py          1.05     17.4±1.05ms     2.3 MB/sec     1.00     16.5±0.47ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.7±0.21ms     3.6 MB/sec     1.00      4.7±0.24ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.11   615.8±41.61µs     4.8 MB/sec     1.00   552.9±24.69µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.06      8.0±0.56ms     3.2 MB/sec     1.00      7.5±0.31ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.17     10.5±0.51ms     3.9 MB/sec     1.00      8.9±0.40ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.07  1959.1±117.99µs     8.5 MB/sec    1.00  1838.1±91.10µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00   225.7±11.43µs    13.1 MB/sec     1.00   225.8±13.74µs    13.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.16ms     6.4 MB/sec     1.00      4.0±0.17ms     6.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-08-04 16:56_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:918 on 2023-08-04 16:56_

I changed this from `parent_scope` to `scope_parent`... it takes a scope and returns its parent. I don't know if this is an improvement, but in a future PR I added `statement_parent` to take a statement and return its parent, and it felt better to have `thing_parent` than multiple `parent_thing` methods.

---

_@MichaReiser reviewed on 2023-08-04 17:10_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:918 on 2023-08-04 17:10_

I prefer `parent_scope`. I find it reads more naturally. I also prefer to name methods after what they return, the arguments only come to play when I want to achieve overloading and need to distinguish the functions. 

Edit: Is there a chance that we can make the naming consistent: Either `scope_current` and `scope_parent` or `current_scope` and `parent_scope`. I prefer the latter.

---

_@MichaReiser approved on 2023-08-04 17:10_

---

_@zanieb reviewed on 2023-08-04 17:12_

---

_Review comment by @zanieb on `crates/ruff_python_semantic/src/model.rs`:918 on 2023-08-04 17:12_

I think `parent_scope()` makes more sense relative to the current scope. `scope_parent` makes more sense when it consumes a scope and gives you _its_ parent 🤷‍♀️ we already went back and forth on this in the pull request that added this though. 

---

_@zanieb reviewed on 2023-08-04 17:12_

---

_Review comment by @zanieb on `crates/ruff/src/checkers/ast/analyze/deferred_for_loops.rs`:19 on 2023-08-04 17:12_

I strongly prefer `current_statement`.

---

_@charliermarsh reviewed on 2023-08-04 17:55_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:918 on 2023-08-04 17:55_

I legitimately forgot that we'd debated this in the prior PR, sorry to bring it up again. I will leave it as `parent_scope` for now, since we seem undecided and I'll err on the side of inertia. 

---

_Merged by @charliermarsh on 2023-08-07 14:44_

---

_Closed by @charliermarsh on 2023-08-07 14:44_

---

_Branch deleted on 2023-08-07 14:44_

---
