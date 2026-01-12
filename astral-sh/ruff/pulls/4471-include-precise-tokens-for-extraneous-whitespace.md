```yaml
number: 4471
title: Include precise tokens for extraneous-whitespace diagnostics
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/bracket
created_at: 2023-05-17T14:46:06Z
updated_at: 2023-05-17T16:45:15Z
url: https://github.com/astral-sh/ruff/pull/4471
synced_at: 2026-01-12T15:55:15Z
```

# Include precise tokens for extraneous-whitespace diagnostics

---

_@charliermarsh_

## Summary

This PR modifies the message such that it shows the actual bracket used (like `[`) instead of always using a parenthesis (like `(`), which is consistent with pycodestyle.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-17 14:46_

---

_@charliermarsh reviewed on 2023-05-17 14:49_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/extraneous_whitespace.rs`:165 on 2023-05-17 14:49_

All this modeling down here is kind of verbose, but does capture the logic and semantics accurately.

---

_Comment by @github-actions[bot] on 2023-05-17 14:55_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.1±0.11ms     2.4 MB/sec    1.00     17.2±0.12ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.02ms     4.0 MB/sec    1.00      4.1±0.01ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    507.6±1.17µs     5.8 MB/sec    1.01    511.1±0.93µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.03ms     3.6 MB/sec    1.00      7.0±0.02ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      8.2±0.03ms     5.0 MB/sec    1.00      8.1±0.02ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1763.9±5.18µs     9.4 MB/sec    1.00   1751.5±5.15µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.3±0.31µs    15.1 MB/sec    1.01    197.6±0.22µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.02ms     6.9 MB/sec    1.00      3.7±0.01ms     7.0 MB/sec
parser/large/dataset.py                    1.00      6.4±0.01ms     6.3 MB/sec    1.01      6.5±0.01ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.00   1259.5±1.20µs    13.2 MB/sec    1.01   1272.3±5.90µs    13.1 MB/sec
parser/numpy/globals.py                    1.00    129.9±0.31µs    22.7 MB/sec    1.01    131.0±0.26µs    22.5 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.00ms     9.3 MB/sec    1.01      2.8±0.00ms     9.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     20.2±0.75ms     2.0 MB/sec    1.11     22.5±1.56ms  1852.0 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.21ms     3.2 MB/sec    1.12      5.8±0.41ms     2.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   604.6±33.77µs     4.9 MB/sec    1.12   677.5±30.29µs     4.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.4±0.35ms     3.0 MB/sec    1.15      9.7±0.38ms     2.6 MB/sec
linter/default-rules/large/dataset.py      1.00     10.6±1.08ms     3.9 MB/sec    1.10     11.6±0.47ms     3.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.11ms     7.9 MB/sec    1.14      2.4±0.09ms     6.9 MB/sec
linter/default-rules/numpy/globals.py      1.12   296.4±27.75µs    10.0 MB/sec    1.00   265.8±18.74µs    11.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.8±0.27ms     5.3 MB/sec    1.05      5.0±0.18ms     5.1 MB/sec
parser/large/dataset.py                    1.00      8.0±0.31ms     5.1 MB/sec    1.14      9.1±0.49ms     4.4 MB/sec
parser/numpy/ctypeslib.py                  1.00  1556.7±76.22µs    10.7 MB/sec    1.15  1788.2±70.14µs     9.3 MB/sec
parser/numpy/globals.py                    1.00    163.9±9.01µs    18.0 MB/sec    1.08   177.0±12.42µs    16.7 MB/sec
parser/pydantic/types.py                   1.00      3.5±0.12ms     7.3 MB/sec    1.13      3.9±0.11ms     6.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/extraneous_whitespace.rs`:165 on 2023-05-17 15:21_

An alternative that removes the need for at least the `OpenBracket`, `CloseBracket` and `Punctuation` enums is to store a `char` in the `BracketOrPunctuation` variants. 

```suggestion
    OpenBracket(char),
    CloseBracket(char)
    Punctuation(char)
```

It's slightly less explicit but I think it would do the job, especially since this is all internal.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/extraneous_whitespace.rs`:136 on 2023-05-17 15:23_

Nit: It would be nice if we wouldn't have to duplicate this logic. Can we maybe move this into a function?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/extraneous_whitespace.rs`:147 on 2023-05-17 15:24_

Unrelated to your change but I think having to check that `prev_token` is not `EOF` is nonsensical... We should be able to just remove it and consider it a lexer bug if the EOF token is ever emitted between two tokens.

---

_@MichaReiser approved on 2023-05-17 15:24_

---

_@charliermarsh reviewed on 2023-05-17 15:30_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/extraneous_whitespace.rs`:165 on 2023-05-17 15:30_

Why are you trying to make invalid states representable???

---

_@MichaReiser reviewed on 2023-05-17 15:35_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/extraneous_whitespace.rs`:165 on 2023-05-17 15:35_

Because I'm lazy and it's private ;)

---

_@charliermarsh reviewed on 2023-05-17 16:14_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/extraneous_whitespace.rs`:147 on 2023-05-17 16:14_

I think it's used in lieu of `None`? Note that we use it at initialization for `prev_token`. I'll change it.

---

_Merged by @charliermarsh on 2023-05-17 16:25_

---

_Closed by @charliermarsh on 2023-05-17 16:25_

---

_Branch deleted on 2023-05-17 16:25_

---
