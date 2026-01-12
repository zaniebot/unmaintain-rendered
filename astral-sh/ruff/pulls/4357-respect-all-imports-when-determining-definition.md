```yaml
number: 4357
title: "Respect `__all__` imports when determining definition visibility"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/__all__
created_at: 2023-05-10T19:10:13Z
updated_at: 2023-05-11T18:01:47Z
url: https://github.com/astral-sh/ruff/pull/4357
synced_at: 2026-01-12T15:55:15Z
```

# Respect `__all__` imports when determining definition visibility

---

_@charliermarsh_

## Summary

When determining whether a function, method, etc. is public or private, we need to take `__all__` into account. This PR leverages our new delayed-visibility logic to respect `__all__` exports.

For more context, see #4339.

Closes #993.


---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/definition.rs`:139 on 2023-05-10 19:22_

I changed this from a stateful iterator to a separate `ContextualizedDefinitions` struct. (We can't use an iterator because of the lifetimes: using `exports` is an immutable borrow of `Context`, and we can't be borrowing `Context` here.)

Using all these structs feels kind of clunky, open to feedback.

---

_@charliermarsh reviewed on 2023-05-10 19:22_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/definition.rs`:139 on 2023-05-11 08:15_

One option could be to simply clone `Exports.names`. It should be relatively cheap because it only runs once and should only contains few entries.

---

_@MichaReiser approved on 2023-05-11 08:16_

---

_Comment by @github-actions[bot] on 2023-05-11 08:52_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.2±0.06ms     2.9 MB/sec    1.01     14.3±0.02ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    355.0±1.31µs     8.3 MB/sec    1.01    358.5±1.72µs     8.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.02ms     4.2 MB/sec    1.01      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.01ms     5.7 MB/sec    1.01      7.2±0.01ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1494.4±3.94µs    11.1 MB/sec    1.01   1513.9±3.30µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    160.9±0.24µs    18.3 MB/sec    1.02    163.7±0.91µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     7.9 MB/sec    1.01      3.3±0.01ms     7.8 MB/sec
parser/large/dataset.py                    1.02      5.9±0.01ms     6.9 MB/sec    1.00      5.8±0.01ms     7.0 MB/sec
parser/numpy/ctypeslib.py                  1.02   1144.0±1.17µs    14.6 MB/sec    1.00   1119.6±0.77µs    14.9 MB/sec
parser/numpy/globals.py                    1.03    117.5±0.31µs    25.1 MB/sec    1.00    114.3±0.33µs    25.8 MB/sec
parser/pydantic/types.py                   1.01      2.5±0.00ms    10.3 MB/sec    1.00      2.4±0.00ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     20.8±0.41ms  2002.6 KB/sec    1.00     20.5±0.41ms  2036.7 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      5.3±0.18ms     3.2 MB/sec    1.00      5.2±0.13ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   605.7±19.40µs     4.9 MB/sec    1.00   607.4±23.42µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.7±0.21ms     2.9 MB/sec    1.00      8.7±0.23ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.03     10.3±0.23ms     4.0 MB/sec    1.00     10.0±0.19ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.07ms     7.6 MB/sec    1.00      2.2±0.15ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   243.0±11.06µs    12.1 MB/sec    1.01   245.9±11.04µs    12.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.7±0.16ms     5.5 MB/sec    1.00      4.6±0.12ms     5.5 MB/sec
parser/large/dataset.py                    1.01      8.2±0.15ms     5.0 MB/sec    1.00      8.1±0.15ms     5.0 MB/sec
parser/numpy/ctypeslib.py                  1.01  1565.4±58.08µs    10.6 MB/sec    1.00  1555.1±54.06µs    10.7 MB/sec
parser/numpy/globals.py                    1.00    157.3±6.36µs    18.8 MB/sec    1.00    157.3±6.07µs    18.8 MB/sec
parser/pydantic/types.py                   1.01      3.5±0.08ms     7.3 MB/sec    1.00      3.4±0.08ms     7.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-05-11 17:43_

---

_Closed by @charliermarsh on 2023-05-11 17:43_

---

_Branch deleted on 2023-05-11 17:43_

---
