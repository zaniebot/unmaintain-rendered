```yaml
number: 5417
title: "Add documentation to `flake8-logging-format` rules"
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: logging-docs
created_at: 2023-06-28T11:52:31Z
updated_at: 2023-07-10T09:54:58Z
url: https://github.com/astral-sh/ruff/pull/5417
synced_at: 2026-01-12T03:36:55Z
```

# Add documentation to `flake8-logging-format` rules

---

_Pull request opened by @tjkuson on 2023-06-28 11:52_

## Summary

Completes the documentation for the `flake8-logging-format` rules. Related to #2646.

I included both the `flake8-logging-format` recommendation to use the `extra` keyword and the Pylint recommendation to pass format values as parameters so that formatting is done lazily, as #970 suggests the Pylint logging rules are covered by this ruleset. Using lazy formatting via parameters is probably more common than avoiding formatting entirely in favour of the `extra` argument, regardless.

## Test Plan

`python scripts/check_docs_formatted.py`


---

_Comment by @github-actions[bot] on 2023-06-28 12:03_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+1, -1, 0 error(s))

<details><summary>airflow (+1, -1)</summary>
<p>

```diff
+ airflow/models/variable.py:239:29: G001 Logging statement uses `str.format`
- airflow/models/variable.py:239:29: G001 Logging statement uses `string.format()`
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| G001 | 2 | 1 | 1 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      9.3±0.36ms     4.4 MB/sec    1.00      9.1±0.31ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.00  1952.7±92.61µs     8.5 MB/sec    1.00  1959.9±78.33µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00   224.1±12.88µs    13.2 MB/sec    1.02   227.5±14.00µs    13.0 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.17ms     6.0 MB/sec    1.03      4.3±0.17ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     15.7±0.46ms     2.6 MB/sec    1.00     15.6±0.48ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.14ms     4.3 MB/sec    1.01      3.9±0.17ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.01   521.9±28.82µs     5.7 MB/sec    1.00   518.1±25.95µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.9±0.24ms     3.7 MB/sec    1.00      6.9±0.21ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.19ms     5.2 MB/sec    1.00      7.9±0.37ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1690.1±51.27µs     9.9 MB/sec    1.01  1709.8±56.94µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   203.2±13.32µs    14.5 MB/sec    1.00   203.9±10.84µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.6±0.12ms     7.1 MB/sec    1.00      3.5±0.11ms     7.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.25     13.4±0.44ms     3.0 MB/sec    1.00     10.7±0.32ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.18      2.7±0.17ms     6.1 MB/sec    1.00      2.3±0.10ms     7.1 MB/sec
formatter/numpy/globals.py                 1.03   302.5±17.49µs     9.8 MB/sec    1.00   293.2±29.97µs    10.1 MB/sec
formatter/pydantic/types.py                1.12      5.9±0.25ms     4.3 MB/sec    1.00      5.3±0.19ms     4.8 MB/sec
linter/all-rules/large/dataset.py          1.00     16.7±0.90ms     2.4 MB/sec    1.20     20.0±1.05ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.8±0.41ms     3.5 MB/sec    1.06      5.1±0.36ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   597.2±24.60µs     4.9 MB/sec    1.05   624.6±22.36µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.5±0.46ms     3.0 MB/sec    1.00      8.5±0.41ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.16     10.3±0.45ms     4.0 MB/sec    1.00      8.9±0.90ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.09      2.1±0.09ms     7.8 MB/sec    1.00  1969.4±106.59µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.04   252.4±10.94µs    11.7 MB/sec    1.00   242.6±11.42µs    12.2 MB/sec
linter/default-rules/pydantic/types.py     1.06      4.4±0.15ms     5.8 MB/sec    1.00      4.1±0.16ms     6.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @dhruvmanila by @charliermarsh on 2023-06-29 01:20_

---

_Label `documentation` added by @charliermarsh on 2023-06-29 01:22_

---

_Merged by @charliermarsh on 2023-06-29 01:30_

---

_Closed by @charliermarsh on 2023-06-29 01:30_

---

_Branch deleted on 2023-07-10 09:54_

---
