```yaml
number: 4819
title: "Move `Binding` initialization into `SemanticModel`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/binding-kind
created_at: 2023-06-02T21:53:32Z
updated_at: 2023-06-03T19:34:40Z
url: https://github.com/astral-sh/ruff/pull/4819
synced_at: 2026-01-12T03:50:03Z
```

# Move `Binding` initialization into `SemanticModel`

---

_Pull request opened by @charliermarsh on 2023-06-02 21:53_

## Summary

Adds some convenience methods to create a new `Binding` in the current semantic context based on a kind and range.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-06-02 21:53_

---

_Comment by @github-actions[bot] on 2023-06-02 22:06_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     18.7±0.69ms     2.2 MB/sec    1.01     18.8±0.70ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.5±0.16ms     3.7 MB/sec    1.00      4.5±0.20ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.01   575.4±24.00µs     5.1 MB/sec    1.00   568.3±34.26µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.0±0.36ms     3.2 MB/sec    1.00      8.0±0.35ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      9.1±0.28ms     4.5 MB/sec    1.00      9.1±0.43ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1969.8±72.05µs     8.5 MB/sec    1.03      2.0±0.11ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.02    240.0±9.76µs    12.3 MB/sec    1.00   235.3±15.47µs    12.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.2±0.17ms     6.1 MB/sec    1.01      4.2±0.26ms     6.0 MB/sec
parser/large/dataset.py                    1.00      6.9±0.22ms     5.9 MB/sec    1.02      7.1±0.24ms     5.7 MB/sec
parser/numpy/ctypeslib.py                  1.00  1397.6±82.16µs    11.9 MB/sec    1.02  1427.2±78.63µs    11.7 MB/sec
parser/numpy/globals.py                    1.00    143.1±9.39µs    20.6 MB/sec    1.02    145.7±8.83µs    20.3 MB/sec
parser/pydantic/types.py                   1.00      3.1±0.12ms     8.1 MB/sec    1.02      3.2±0.15ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.1±0.24ms     2.4 MB/sec    1.02     17.4±0.33ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.05ms     3.9 MB/sec    1.00      4.3±0.08ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   507.6±10.23µs     5.8 MB/sec    1.00    507.3±7.95µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.11ms     3.5 MB/sec    1.01      7.3±0.12ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.10ms     4.8 MB/sec    1.02      8.5±0.09ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1775.8±19.53µs     9.4 MB/sec    1.02  1813.3±24.23µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    205.2±3.87µs    14.4 MB/sec    1.01    207.7±6.45µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.05ms     6.7 MB/sec    1.01      3.8±0.05ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.4±0.04ms     6.3 MB/sec    1.00      6.4±0.04ms     6.4 MB/sec
parser/numpy/ctypeslib.py                  1.01  1219.6±15.07µs    13.7 MB/sec    1.00  1205.5±14.56µs    13.8 MB/sec
parser/numpy/globals.py                    1.02    125.9±2.83µs    23.4 MB/sec    1.00    123.0±2.16µs    24.0 MB/sec
parser/pydantic/types.py                   1.01      2.8±0.03ms     9.3 MB/sec    1.00      2.7±0.03ms     9.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:4474 on 2023-06-02 22:08_

This is cleaner but does mean we're doing the scope lookup every time.

---

_@charliermarsh reviewed on 2023-06-02 22:08_

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:267 on 2023-06-03 14:05_

What's the reason that `scoped_binding` does not perform the `bindings.push` operation? Are there cases where we don't want to push the binding to bindings?

---

_@MichaReiser approved on 2023-06-03 14:05_

---

_@charliermarsh reviewed on 2023-06-03 19:05_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:267 on 2023-06-03 19:05_

Let me revisit this in a separate PR. There's some complicated stuff that happens in `add_binding` whereby we need to determine whether a new binding is shadowing an existing binding, and so we need to create the binding in order to do that check, but can't push it into the scope yet. I'd like to move that entire thing into the semantic model.

---

_Merged by @charliermarsh on 2023-06-03 19:26_

---

_Closed by @charliermarsh on 2023-06-03 19:26_

---

_Branch deleted on 2023-06-03 19:26_

---
