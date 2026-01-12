```yaml
number: 4686
title: "Remove globals table from `Scope`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/globals
created_at: 2023-05-27T20:02:46Z
updated_at: 2023-05-28T02:48:45Z
url: https://github.com/astral-sh/ruff/pull/4686
synced_at: 2026-01-12T03:50:03Z
```

# Remove globals table from `Scope`

---

_Pull request opened by @charliermarsh on 2023-05-27 20:02_

## Summary

This is a follow-up to #4648 to further reduce the size of `Scope`.

At present, on each `Scope`, we store an optional `globals` hash map, which enumerates all the symbols declared as "global" in the scope. (We need to store these persistently and prior to traversal, since as we encountered bindings through the scope body, we need to know if they're declared as global _later on_.) Most scopes don't have any globals, so storing a hash map per scope unnecessarily increases the size of a very common struct.

This PR instead uses an arena to store a vector of hash maps, and stores the `Option<GlobalsId>` on `Scope` instead, as suggested by @MichaReiser in the aforementioned PR. It should reduce the size by 40 bytes per allocation.


---

_@charliermarsh reviewed on 2023-05-27 20:03_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:554 on 2023-05-27 20:03_

`set_*` feels bad, it'd be nice to instead provide these when we push the `Scope`, but that API only accepts a `ScopeKind` right now.

---

_@charliermarsh reviewed on 2023-05-27 20:03_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/globals.rs`:41 on 2023-05-27 20:03_

I may have over-abstracted this a bit, here + with `GlobalsBuilder`? Curious to get reactions.

---

_@MichaReiser reviewed on 2023-05-27 20:12_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/globals.rs`:41 on 2023-05-27 20:12_

I think it makes sense. I would rename it to GlobalsVisitor because it implements visitor and doesn't really provide builder methods (other than finish)

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/globals.rs`:44 on 2023-05-27 20:14_

Nit: what do you think to returning an option here that is none if there are no globals? Would it allow to remove len and is empty?

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:577 on 2023-05-27 20:16_

Nit: rename to 'global'? The rust convention is to prefix the setter with set but use the name alone for the getter

---

_@MichaReiser approved on 2023-05-27 20:17_

Neat. Let's see if this improves performance 

---

_Comment by @github-actions[bot] on 2023-05-27 20:30_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.1±0.05ms     2.9 MB/sec    1.00     14.0±0.10ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    425.5±0.82µs     6.9 MB/sec    1.00    423.4±0.53µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.01ms     4.3 MB/sec    1.00      5.9±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.01ms     6.1 MB/sec    1.02      6.8±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1485.3±1.56µs    11.2 MB/sec    1.02   1510.0±3.37µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    167.5±0.25µs    17.6 MB/sec    1.03    171.8±0.98µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.02      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.2±0.00ms     7.9 MB/sec    1.16      6.0±0.01ms     6.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1021.8±0.41µs    16.3 MB/sec    1.13   1153.1±0.46µs    14.4 MB/sec
parser/numpy/globals.py                    1.00    106.0±1.11µs    27.8 MB/sec    1.09    115.9±0.29µs    25.5 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.4 MB/sec    1.13      2.5±0.01ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.1±0.32ms     2.4 MB/sec    1.00     17.1±0.24ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.08ms     3.9 MB/sec    1.01      4.3±0.10ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.02   512.5±11.23µs     5.8 MB/sec    1.00    503.1±6.84µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.3±0.17ms     3.5 MB/sec    1.00      7.2±0.17ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.11ms     4.8 MB/sec    1.00      8.4±0.14ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1798.2±30.67µs     9.3 MB/sec    1.00  1782.4±23.68µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    204.5±4.90µs    14.4 MB/sec    1.03   211.2±12.89µs    14.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.06ms     6.8 MB/sec    1.01      3.8±0.07ms     6.7 MB/sec
parser/large/dataset.py                    1.01      6.5±0.08ms     6.2 MB/sec    1.00      6.5±0.07ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.01  1243.4±17.61µs    13.4 MB/sec    1.00  1231.2±28.31µs    13.5 MB/sec
parser/numpy/globals.py                    1.01    128.0±9.76µs    23.1 MB/sec    1.00    126.5±2.74µs    23.3 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.06ms     9.1 MB/sec    1.00      2.8±0.07ms     9.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/globals.rs`:41 on 2023-05-27 21:08_

Java Java Java

---

_@charliermarsh reviewed on 2023-05-27 21:08_

---

_@charliermarsh reviewed on 2023-05-27 21:09_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/globals.rs`:41 on 2023-05-27 21:09_

Builder all the things

---

_Merged by @charliermarsh on 2023-05-28 02:35_

---

_Closed by @charliermarsh on 2023-05-28 02:35_

---

_Branch deleted on 2023-05-28 02:35_

---
