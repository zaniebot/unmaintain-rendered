```yaml
number: 4750
title: "Migrate flake8_pyi_rules from `unspecified` to `suggested` and `automatic`"
type: pull_request
state: merged
author: sladyn98
labels: []
assignees: []
merged: true
base: main
head: migrate-flake8py-rules
created_at: 2023-05-31T09:05:16Z
updated_at: 2023-06-01T23:01:16Z
url: https://github.com/astral-sh/ruff/pull/4750
synced_at: 2026-01-12T03:50:03Z
```

# Migrate flake8_pyi_rules from `unspecified` to `suggested` and `automatic`

---

_Pull request opened by @sladyn98 on 2023-05-31 09:05_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Migrates autofix rules from `unspecified` to `suggested` and `automatic`

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Ran all tests 
<!-- How was it tested? -->


---

_@sladyn98 reviewed on 2023-05-31 09:06_

---

_Review comment by @sladyn98 on `crates/ruff/src/rules/flake8_pyi/rules/simple_defaults.rs`:404 on 2023-05-31 09:06_

Applying the fix suggested by the "argument_simple_defaults" lint rule and using simple default values is generally considered safe and recommended. However, it's important to consider the specific requirements and design of the code before applying this rule. In some cases, using mutable default values may be necessary or desired hence I went with suggested here

---

_Renamed from "Migrate flake8_pyi_rules from unspecified to suggested and automatic" to "Migrate flake8_pyi_rules from `unspecified` to `suggested` and `automatic`" by @sladyn98 on 2023-05-31 09:06_

---

_Comment by @github-actions[bot] on 2023-05-31 09:37_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.1±0.06ms     2.9 MB/sec    1.01     14.2±0.09ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.01      3.4±0.03ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    424.8±0.60µs     6.9 MB/sec    1.00    426.0±0.59µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.02ms     4.4 MB/sec    1.01      5.9±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.02ms     6.0 MB/sec    1.02      6.9±0.01ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1498.6±1.73µs    11.1 MB/sec    1.01   1517.7±3.31µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.5±0.21µs    17.3 MB/sec    1.01    172.4±0.44µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.2 MB/sec    1.01      3.1±0.01ms     8.1 MB/sec
parser/large/dataset.py                    1.14      6.1±0.00ms     6.7 MB/sec    1.00      5.3±0.01ms     7.6 MB/sec
parser/numpy/ctypeslib.py                  1.12   1162.3±1.03µs    14.3 MB/sec    1.00   1039.1±1.73µs    16.0 MB/sec
parser/numpy/globals.py                    1.09    116.3±0.26µs    25.4 MB/sec    1.00    107.2±1.49µs    27.5 MB/sec
parser/pydantic/types.py                   1.13      2.6±0.00ms     9.9 MB/sec    1.00      2.3±0.00ms    11.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.7±0.09ms     2.4 MB/sec    1.00     16.7±0.10ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.02ms     3.9 MB/sec    1.00      4.3±0.02ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   442.7±16.58µs     6.7 MB/sec    1.01   447.7±15.95µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.2±0.03ms     3.6 MB/sec    1.00      7.1±0.02ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.4±0.03ms     4.8 MB/sec    1.00      8.4±0.05ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1770.5±8.24µs     9.4 MB/sec    1.01  1788.8±10.98µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    194.4±1.19µs    15.2 MB/sec    1.02    198.1±5.23µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.02ms     6.6 MB/sec    1.00      3.9±0.04ms     6.6 MB/sec
parser/large/dataset.py                    1.02      6.7±0.02ms     6.0 MB/sec    1.00      6.6±0.03ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.02  1276.0±12.18µs    13.0 MB/sec    1.00   1247.7±8.53µs    13.3 MB/sec
parser/numpy/globals.py                    1.03    133.4±0.91µs    22.1 MB/sec    1.00    130.1±0.75µs    22.7 MB/sec
parser/pydantic/types.py                   1.02      2.9±0.01ms     8.9 MB/sec    1.00      2.8±0.03ms     9.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @sladyn98 on 2023-06-01 20:14_

@charliermarsh Would love some 👀 on this . Thanks in advance

---

_Merged by @charliermarsh on 2023-06-01 22:35_

---

_Closed by @charliermarsh on 2023-06-01 22:35_

---
