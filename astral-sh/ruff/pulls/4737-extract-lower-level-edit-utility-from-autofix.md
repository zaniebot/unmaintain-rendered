```yaml
number: 4737
title: Extract lower-level edit utility from autofix module
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/codegen
created_at: 2023-05-30T22:13:02Z
updated_at: 2023-05-31T17:13:47Z
url: https://github.com/astral-sh/ruff/pull/4737
synced_at: 2026-01-12T03:50:03Z
```

# Extract lower-level edit utility from autofix module

---

_Pull request opened by @charliermarsh on 2023-05-30 22:13_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

For flake8-type-checking autofixes, I need to have access to a `remove_imports`-like method that takes a `Stmt` and returns code as a string (rather than an edit). This PR removes that functionality from the `autofix::edits` API (formerly, `autofix::actions`) and instead puts it in a lower-level utility module (`autofix::codemods`).


---

_@charliermarsh reviewed on 2023-05-30 22:13_

---

_Review comment by @charliermarsh on `crates/ruff/src/autofix/edits.rs`:184 on 2023-05-30 22:13_

(I moved these private functions to the bottom of the module.)

---

_Comment by @github-actions[bot] on 2023-05-30 22:29_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.4±0.09ms     2.8 MB/sec    1.00     14.3±0.11ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    423.8±0.57µs     7.0 MB/sec    1.00    423.5±1.04µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.04ms     4.3 MB/sec    1.00      5.9±0.03ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.02      6.9±0.02ms     5.9 MB/sec    1.00      6.8±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1503.9±5.94µs    11.1 MB/sec    1.00   1491.8±3.74µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.1±0.30µs    17.3 MB/sec    1.00    170.5±1.94µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.02ms     8.2 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
parser/large/dataset.py                    1.00      5.3±0.00ms     7.7 MB/sec    1.00      5.3±0.00ms     7.7 MB/sec
parser/numpy/ctypeslib.py                  1.00   1033.0±0.67µs    16.1 MB/sec    1.00   1033.6±1.17µs    16.1 MB/sec
parser/numpy/globals.py                    1.00    107.2±0.57µs    27.5 MB/sec    1.00    106.9±0.16µs    27.6 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.3 MB/sec    1.00      2.3±0.00ms    11.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.1±0.34ms     2.4 MB/sec    1.01     17.2±0.17ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.17      5.2±1.87ms     3.2 MB/sec    1.00      4.4±0.09ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    449.7±6.38µs     6.6 MB/sec    1.02    457.5±7.93µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.4±0.40ms     3.4 MB/sec    1.00      7.3±0.11ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.09ms     4.8 MB/sec    1.01      8.7±0.08ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1795.2±15.31µs     9.3 MB/sec    1.02  1837.2±18.04µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    197.8±2.25µs    14.9 MB/sec    1.01    198.8±3.36µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.04ms     6.6 MB/sec    1.01      3.9±0.03ms     6.5 MB/sec
parser/large/dataset.py                    1.00      6.7±0.03ms     6.1 MB/sec    1.00      6.7±0.04ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1257.1±10.58µs    13.2 MB/sec    1.00  1257.3±10.06µs    13.2 MB/sec
parser/numpy/globals.py                    1.00    130.6±0.86µs    22.6 MB/sec    1.00    130.6±0.98µs    22.6 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.02ms     9.0 MB/sec    1.00      2.8±0.04ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/autofix/codemods.rs`:17 on 2023-05-31 06:43_

Is the reason that this returns a `String` rather than an `Edit` that it may remove the entire import section? 

---

_Review comment by @MichaReiser on `crates/ruff/src/autofix/codemods.rs`:1 on 2023-05-31 06:46_

I would struggle to know whether I'm supposed to put some "mutation" method into the `edits` or `codemod` file. What's your reasoning for separating the too? Does it matter what they are? Should we instead split the mutations by what they're mutating. E.g. `autofix/import` `autofix/arguments`, `autofix/statement`? I would find it easier to know where to place my mutation if the mutations are grouped by the node because I ultimately know on which node my mutation operates. 

---

_@MichaReiser approved on 2023-05-31 06:47_

---

_@charliermarsh reviewed on 2023-05-31 16:41_

---

_Review comment by @charliermarsh on `crates/ruff/src/autofix/codemods.rs`:17 on 2023-05-31 16:41_

It's because we may need access to the modified code for some operations, and `Edit` obscures the actual modified code.

---

_@charliermarsh reviewed on 2023-05-31 16:42_

---

_Review comment by @charliermarsh on `crates/ruff/src/autofix/codemods.rs`:1 on 2023-05-31 16:42_

It's because, for flake8-type-checking, we need access to the modified code, in order to build up the actual edit. If this returned an edit, we wouldn't be able to use it to (e.g.) remove imports from a statement, then place the modified statement within a separate block of code -- since we'd only have the `Edit`.

---

_Merged by @charliermarsh on 2023-05-31 16:50_

---

_Closed by @charliermarsh on 2023-05-31 16:50_

---

_Branch deleted on 2023-05-31 16:50_

---
