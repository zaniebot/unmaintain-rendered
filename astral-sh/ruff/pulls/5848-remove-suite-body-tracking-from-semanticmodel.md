```yaml
number: 5848
title: "Remove suite body tracking from `SemanticModel`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/body
created_at: 2023-07-17T23:47:08Z
updated_at: 2023-07-18T22:58:32Z
url: https://github.com/astral-sh/ruff/pull/5848
synced_at: 2026-01-12T15:55:19Z
```

# Remove suite body tracking from `SemanticModel`

---

_@charliermarsh_

## Summary

The `SemanticModel` currently stores the "body" of a given `Suite`, along with the current statement index. This is used to support "next sibling" queries, but we only use this in exactly one place -- the rule that simplifies constructs like this to `any` or `all`:

```python
for x in y:
    if x == 0:
        return True
return False
```

Instead of tracking the state, we can just do a (slightly more expensive) traversal, by finding the node within its parent and returning the next node in the body.

Note that we'll only have to do this extremely rarely -- namely, for functions that contain something like:

```python
for x in y:
    if x == 0:
        return True
```


---

_Renamed from "Remove body from semantic model" to "Remove suite body tracking from `SemanticModel`" by @charliermarsh on 2023-07-17 23:47_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-17 23:47_

---

_Label `internal` added by @charliermarsh on 2023-07-17 23:47_

---

_Comment by @github-actions[bot] on 2023-07-18 00:01_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.4±0.05ms     4.3 MB/sec    1.00      9.4±0.05ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1860.3±1.44µs     9.0 MB/sec    1.00   1869.4±9.31µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    207.7±0.28µs    14.2 MB/sec    1.00    208.6±0.25µs    14.1 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.01ms     6.3 MB/sec    1.00      4.0±0.01ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.01     13.2±0.10ms     3.1 MB/sec    1.00     13.1±0.10ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.02ms     5.0 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    435.5±2.54µs     6.8 MB/sec    1.01    440.7±0.75µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.0±0.10ms     4.3 MB/sec    1.00      5.9±0.02ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.01ms     6.2 MB/sec    1.01      6.7±0.02ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1411.1±2.08µs    11.8 MB/sec    1.02   1432.5±5.03µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    155.1±0.45µs    19.0 MB/sec    1.03    159.8±1.11µs    18.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.6 MB/sec    1.00      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.0±0.05ms     3.7 MB/sec    1.15     12.7±0.22ms     3.2 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.02ms     7.8 MB/sec    1.11      2.4±0.01ms     7.0 MB/sec
formatter/numpy/globals.py                 1.00    231.6±3.04µs    12.7 MB/sec    1.07    248.4±3.74µs    11.9 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.05ms     5.4 MB/sec    1.12      5.3±0.05ms     4.8 MB/sec
linter/all-rules/large/dataset.py          1.03     15.6±0.29ms     2.6 MB/sec    1.00     15.2±0.09ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.1±0.04ms     4.1 MB/sec    1.00      4.0±0.02ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    418.1±5.29µs     7.1 MB/sec    1.00    415.2±5.27µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.0±0.05ms     3.6 MB/sec    1.00      6.9±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.1±0.04ms     5.0 MB/sec    1.00      8.0±0.11ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1637.7±16.03µs    10.2 MB/sec    1.00  1628.5±16.02µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    172.9±1.26µs    17.1 MB/sec    1.00    172.2±3.31µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.05ms     7.1 MB/sec    1.01      3.6±0.17ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @MichaReiser on 2023-07-18 06:05_

Can you add your benchmark numbers from running only the affected rule in the CPython repository before and after? To get an idea of how this affects performance. 

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/traversal.rs`:100 on 2023-07-18 06:06_

Nit: Listing all no-op statements explicitly would ensure that rust guides engineers adding a new statement. 

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/traversal.rs`:6 on 2023-07-18 06:07_

Nit: `containing_suite`? 

This is one of the places where a `SuiteStmt` would help a lot because the parent would then be the suite directly. 

Would it make sense to not only search for the statement, but also its index to avoid two passes? 

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:4244 on 2023-07-18 06:09_

Nit: Probably not worth doing. We could make this approach slightly faster if we track the *branch* we're currently in. This should be less problematic because we only need to write the data when we take a new branch. Don't we already have something similar? 

---

_@MichaReiser approved on 2023-07-18 06:10_

---

_@charliermarsh reviewed on 2023-07-18 14:00_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/traversal.rs`:100 on 2023-07-18 14:00_

Do you think we should bias towards this pattern (explicit enumeration of AST nodes) everywhere? Or use it selectively?

---

_@MichaReiser reviewed on 2023-07-18 15:20_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/traversal.rs`:100 on 2023-07-18 15:20_

I use it selectively based on the use case. I use this pattern when I realize that I start by generating all match cases and remove the ones I don't care about because I can't easily tell which nodes should be handled. 

Another way to think about this. Would an author adding a new statement know that they need to add it here? If not, then it could make sense to use an exhaustive match

---

_@charliermarsh reviewed on 2023-07-18 15:27_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/traversal.rs`:6 on 2023-07-18 15:27_

Yeah, maybe I'll return a struct here with a method to grab the next sibling.

---

_@charliermarsh reviewed on 2023-07-18 15:27_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/traversal.rs`:100 on 2023-07-18 15:27_

Makes sense

---

_Comment by @charliermarsh on 2023-07-18 22:57_

I spent some time trying to benchmark this. `cargo bench` shows no significant difference, but I don't think any of those files _actually_ trigger this rule. So I then added a dedicated file from CPython that _does_ trigger it, and again, no noticeable difference.


---

_Comment by @charliermarsh on 2023-07-18 22:58_

I have a lot of trouble running the CPython benchmark for specific rules because the timing is completely dominated by everything else.

---

_Merged by @charliermarsh on 2023-07-18 22:58_

---

_Closed by @charliermarsh on 2023-07-18 22:58_

---

_Branch deleted on 2023-07-18 22:58_

---
