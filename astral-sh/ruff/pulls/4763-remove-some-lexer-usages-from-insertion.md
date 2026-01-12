```yaml
number: 4763
title: "Remove some lexer usages from `Insertion`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/insertions
created_at: 2023-05-31T18:25:11Z
updated_at: 2023-06-01T20:08:55Z
url: https://github.com/astral-sh/ruff/pull/4763
synced_at: 2026-01-12T03:50:03Z
```

# Remove some lexer usages from `Insertion`

---

_Pull request opened by @charliermarsh on 2023-05-31 18:25_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Some follow-ups to feedback given in #4741.

## Test Plan

`cargo test`


---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-31 18:25_

---

_Comment by @github-actions[bot] on 2023-05-31 18:34_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     15.1±0.07ms     2.7 MB/sec    1.00     15.0±0.11ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.01ms     4.6 MB/sec    1.00      3.7±0.01ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    376.4±1.17µs     7.8 MB/sec    1.00    378.2±1.50µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.02ms     4.0 MB/sec    1.00      6.3±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.06ms     5.4 MB/sec    1.00      7.5±0.02ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1605.5±3.99µs    10.4 MB/sec    1.00   1613.0±4.09µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    177.1±1.00µs    16.7 MB/sec    1.02    180.1±3.31µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.06ms     7.5 MB/sec    1.00      3.4±0.02ms     7.5 MB/sec
parser/large/dataset.py                    1.00      5.7±0.01ms     7.1 MB/sec    1.00      5.7±0.03ms     7.1 MB/sec
parser/numpy/ctypeslib.py                  1.00   1123.2±1.08µs    14.8 MB/sec    1.00   1126.3±1.67µs    14.8 MB/sec
parser/numpy/globals.py                    1.00    115.5±1.14µs    25.6 MB/sec    1.00    116.0±0.53µs    25.4 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.4 MB/sec    1.00      2.5±0.00ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.4±0.10ms     2.5 MB/sec    1.00     16.3±0.10ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.04ms     4.0 MB/sec    1.00      4.2±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    494.9±5.55µs     6.0 MB/sec    1.01    497.6±9.11µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.05ms     3.7 MB/sec    1.00      6.9±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.05ms     5.0 MB/sec    1.00      8.1±0.04ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1750.8±17.15µs     9.5 MB/sec    1.00  1745.1±25.19µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    202.8±3.33µs    14.6 MB/sec    1.01    204.3±8.41µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.03ms     6.9 MB/sec    1.00      3.7±0.03ms     6.9 MB/sec
parser/large/dataset.py                    1.00      6.3±0.04ms     6.4 MB/sec    1.02      6.5±0.04ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1188.8±10.80µs    14.0 MB/sec    1.02  1208.1±11.14µs    13.8 MB/sec
parser/numpy/globals.py                    1.00    122.6±1.68µs    24.1 MB/sec    1.02    124.9±1.84µs    23.6 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.03ms     9.5 MB/sec    1.02      2.7±0.05ms     9.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/importer/insertion.rs`:73 on 2023-06-01 06:19_

Line exposes the `start` and end offsets to avoid re-scanning the text to find the end of the line

```suggestion
                location = line.full_end();
```

Is it okay that `location` now is a relative offset? If not, use UniversalNewlinesIter::with_offset(locator.after(location), location)`

---

_Review comment by @MichaReiser on `crates/ruff/src/importer/insertion.rs`:292 on 2023-06-01 06:22_

Is it okay that this returns a relative offset from the beginning of the string?

---

_@MichaReiser approved on 2023-06-01 06:23_

---

_@charliermarsh reviewed on 2023-06-01 19:36_

---

_Review comment by @charliermarsh on `crates/ruff/src/importer/insertion.rs`:292 on 2023-06-01 19:36_

Yup!

---

_@charliermarsh reviewed on 2023-06-01 19:37_

---

_Review comment by @charliermarsh on `crates/ruff/src/importer/insertion.rs`:73 on 2023-06-01 19:37_

Thanks, fixed.

---

_Merged by @charliermarsh on 2023-06-01 19:45_

---

_Closed by @charliermarsh on 2023-06-01 19:45_

---

_Branch deleted on 2023-06-01 19:45_

---
