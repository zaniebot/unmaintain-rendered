```yaml
number: 4734
title: "Remove unused `getrandom` dependency"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/udeps
created_at: 2023-05-30T17:34:46Z
updated_at: 2023-05-30T19:22:39Z
url: https://github.com/astral-sh/ruff/pull/4734
synced_at: 2026-01-12T03:50:03Z
```

# Remove unused `getrandom` dependency

---

_Pull request opened by @charliermarsh on 2023-05-30 17:34_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This is being flagged by the `udeps` job, though not sure why it's only now being marked as unused?


---

_Comment by @github-actions[bot] on 2023-05-30 17:51_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     19.0±0.63ms     2.1 MB/sec    1.00     18.9±0.68ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.18ms     3.8 MB/sec    1.01      4.5±0.19ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   551.3±18.38µs     5.4 MB/sec    1.03   565.2±32.07µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.8±0.25ms     3.3 MB/sec    1.00      7.6±0.21ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.02      8.9±0.36ms     4.6 MB/sec    1.00      8.7±0.27ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1927.7±68.96µs     8.6 MB/sec    1.00  1903.0±114.95µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   225.3±10.07µs    13.1 MB/sec    1.02   229.8±10.53µs    12.8 MB/sec
linter/default-rules/pydantic/types.py     1.04      4.0±0.19ms     6.3 MB/sec    1.00      3.9±0.08ms     6.6 MB/sec
parser/large/dataset.py                    1.00      6.6±0.20ms     6.2 MB/sec    1.00      6.6±0.24ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1294.7±44.52µs    12.9 MB/sec    1.00  1290.8±33.10µs    12.9 MB/sec
parser/numpy/globals.py                    1.00    129.3±4.87µs    22.8 MB/sec    1.01    130.4±5.43µs    22.6 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.11ms     8.9 MB/sec    1.00      2.9±0.09ms     8.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     19.7±0.47ms     2.1 MB/sec    1.00     19.7±0.45ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.9±0.11ms     3.4 MB/sec    1.01      5.0±0.18ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.05   617.9±50.89µs     4.8 MB/sec    1.00   590.6±24.22µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.3±0.28ms     3.1 MB/sec    1.00      8.3±0.26ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.7±0.23ms     4.2 MB/sec    1.01      9.8±0.23ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.06ms     8.1 MB/sec    1.03      2.1±0.09ms     7.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    241.1±9.76µs    12.2 MB/sec    1.01   243.4±17.43µs    12.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.4±0.29ms     5.8 MB/sec    1.02      4.5±0.17ms     5.7 MB/sec
parser/large/dataset.py                    1.11      8.4±0.35ms     4.8 MB/sec    1.00      7.6±0.17ms     5.4 MB/sec
parser/numpy/ctypeslib.py                  1.03  1475.9±58.36µs    11.3 MB/sec    1.00  1439.4±45.86µs    11.6 MB/sec
parser/numpy/globals.py                    1.01    148.6±7.66µs    19.9 MB/sec    1.00    147.1±4.79µs    20.1 MB/sec
parser/pydantic/types.py                   1.04      3.3±0.28ms     7.7 MB/sec    1.00      3.2±0.10ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser reviewed on 2023-05-30 18:25_

---

_Review comment by @MichaReiser on `crates/ruff_wasm/Cargo.toml`:23 on 2023-05-30 18:25_

Hmm that's strange. It used to be required for the wasm project to build correctly

---

_@MichaReiser approved on 2023-05-30 18:25_

---

_Merged by @charliermarsh on 2023-05-30 18:34_

---

_Closed by @charliermarsh on 2023-05-30 18:34_

---

_Branch deleted on 2023-05-30 18:34_

---

_@charliermarsh reviewed on 2023-05-30 18:43_

---

_Review comment by @charliermarsh on `crates/ruff_wasm/Cargo.toml`:23 on 2023-05-30 18:43_

Given that we don't understand this, I'm assuming something will break :joy:

---

_Comment by @MichaReiser on 2023-05-30 19:19_

@charliermarsh this breaks

```bash
cargo test --target wasm32-unknown-unknown
```

Edit: never mind. You're supposed to use `wasm-pack test --node` and that continues to work. 

---
