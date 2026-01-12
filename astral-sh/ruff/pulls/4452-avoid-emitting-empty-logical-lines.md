```yaml
number: 4452
title: Avoid emitting empty logical lines
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/empty
created_at: 2023-05-16T15:25:33Z
updated_at: 2023-05-16T16:55:52Z
url: https://github.com/astral-sh/ruff/pull/4452
synced_at: 2026-01-12T15:55:15Z
```

# Avoid emitting empty logical lines

---

_@charliermarsh_

## Summary

The changes to the lexer reflected in #4444 cause us to now encounter empty lines in the token stream, which we're now emitting as logical lines. Pycodestyle skips these entirely, and tracks the "number of blank lines" separately. This PR mimics that behavior, skipping empty lines.

(We may want to make this a flag on `LogicalLine`, but this seems fine for now.)


---

_@charliermarsh reviewed on 2023-05-16 15:25_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/pycodestyle/E11.py`:66 on 2023-05-16 15:25_

We flag this as E112 (expected an indented block), but pycodestyle does not.

---

_Comment by @charliermarsh on 2023-05-16 15:31_

One bug, fixing...

---

_Comment by @github-actions[bot] on 2023-05-16 15:34_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     18.0±0.43ms     2.3 MB/sec    1.00     18.0±0.32ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.3±0.35ms     3.8 MB/sec    1.00      4.3±0.08ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01   545.5±16.71µs     5.4 MB/sec    1.00   539.9±11.06µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.4±0.16ms     3.5 MB/sec    1.00      7.4±0.15ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.13ms     4.8 MB/sec    1.00      8.4±0.09ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1790.5±37.14µs     9.3 MB/sec    1.01  1804.6±30.76µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.01    216.5±8.36µs    13.6 MB/sec    1.00    215.2±6.13µs    13.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.10ms     6.7 MB/sec    1.00      3.8±0.06ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.7±0.10ms     6.1 MB/sec    1.00      6.7±0.18ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.01  1326.9±27.51µs    12.5 MB/sec    1.00  1313.8±25.64µs    12.7 MB/sec
parser/numpy/globals.py                    1.01    134.5±4.37µs    21.9 MB/sec    1.00    132.9±3.93µs    22.2 MB/sec
parser/pydantic/types.py                   1.01      2.9±0.08ms     8.8 MB/sec    1.00      2.9±0.06ms     8.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     20.8±1.42ms  2006.2 KB/sec    1.10     22.8±1.31ms  1824.3 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.5±0.38ms     3.0 MB/sec    1.00      5.5±0.54ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.02   633.1±44.41µs     4.7 MB/sec    1.00   617.7±60.73µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.0±0.51ms     2.8 MB/sec    1.02      9.2±0.56ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.03     10.6±0.60ms     3.9 MB/sec    1.00     10.3±0.51ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.3±0.10ms     7.3 MB/sec    1.00      2.2±0.14ms     7.5 MB/sec
linter/default-rules/numpy/globals.py      1.01   272.1±18.71µs    10.8 MB/sec    1.00   269.5±25.97µs    10.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.9±0.31ms     5.2 MB/sec    1.04      5.1±0.45ms     5.0 MB/sec
parser/large/dataset.py                    1.00      8.6±0.38ms     4.7 MB/sec    1.03      8.9±0.45ms     4.6 MB/sec
parser/numpy/ctypeslib.py                  1.00  1610.0±93.40µs    10.3 MB/sec    1.08  1737.5±91.22µs     9.6 MB/sec
parser/numpy/globals.py                    1.00   168.3±12.31µs    17.5 MB/sec    1.06   179.1±12.05µs    16.5 MB/sec
parser/pydantic/types.py                   1.00      3.6±0.17ms     7.0 MB/sec    1.05      3.8±0.20ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/mod.rs`:106 on 2023-05-16 15:41_

```suggestion
                TokenKind::NonLogicalNewline if parens == 0 && non_empty => { 
              		builder.finish_line();
```

---

_@MichaReiser reviewed on 2023-05-16 15:41_

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/logical_lines.rs`:267 on 2023-05-16 15:50_

Nit: Can we split this into its own test. Giving tests an explicit name help future us because it gives another hint on what the test is asserting and also allows to run the test on its own.

---

_@charliermarsh reviewed on 2023-05-16 15:57_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/mod.rs`:106 on 2023-05-16 15:57_

Moved the checks into the builder instead, which is necessary to handle `builder.finish()` properly (which calls `.finish_line()` internally).

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/mod.rs`:461 on 2023-05-16 15:59_

Do you think it would work if we instead test inside of `finish_line` if all `tokens` are `NonLogicalNewLine`? That would have the benefit that it only runs once per line, doesn't require more state, and doesn't introduce more branching in this hot path (at the cost that, in the worst case, we have to iterate over all line tokens). We could optimize this further by providing a `finish_non_logical_line` and only add the check there (calls into the `finish_line`

---

_@MichaReiser approved on 2023-05-16 15:59_

Looks good. I only have some nits 

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/logical_lines.rs`:267 on 2023-05-16 16:28_

Gonna do this as its own PR.

---

_@charliermarsh reviewed on 2023-05-16 16:28_

---

_Merged by @charliermarsh on 2023-05-16 16:33_

---

_Closed by @charliermarsh on 2023-05-16 16:33_

---

_Branch deleted on 2023-05-16 16:33_

---
