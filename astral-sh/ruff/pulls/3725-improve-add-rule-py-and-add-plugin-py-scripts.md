```yaml
number: 3725
title: Improve add_rule.py and add_plugin.py scripts
type: pull_request
state: merged
author: JonathanPlasse
labels:
  - internal
assignees: []
merged: true
base: main
head: improve-add-rule-script
created_at: 2023-03-24T22:02:06Z
updated_at: 2023-03-25T16:45:14Z
url: https://github.com/astral-sh/ruff/pull/3725
synced_at: 2026-01-12T04:39:45Z
```

# Improve add_rule.py and add_plugin.py scripts

---

_Pull request opened by @JonathanPlasse on 2023-03-24 22:02_

Will sort the plugin section if unsorted.
`add_rule.py` now separate `--code` into `--prefix` and `--code`.

---

_Comment by @github-actions[bot] on 2023-03-24 22:46_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.8±0.07ms     3.0 MB/sec    1.00     13.8±0.12ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.02ms     4.6 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.03    501.9±2.39µs     5.9 MB/sec    1.00    488.9±0.87µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.01ms     4.2 MB/sec    1.00      6.0±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.03ms     5.6 MB/sec    1.01      7.4±0.05ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1647.8±2.21µs    10.1 MB/sec    1.00   1644.2±4.71µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    183.3±0.65µs    16.1 MB/sec    1.00    184.0±1.21µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.4 MB/sec    1.00      3.4±0.02ms     7.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     19.9±0.69ms     2.0 MB/sec    1.03     20.6±0.74ms  2026.2 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.24ms     3.2 MB/sec    1.02      5.3±0.23ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   646.2±27.95µs     4.6 MB/sec    1.01   655.5±27.69µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.6±0.30ms     3.0 MB/sec    1.03      8.8±0.41ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00     10.7±0.29ms     3.8 MB/sec    1.06     11.4±0.42ms     3.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.4±0.10ms     7.0 MB/sec    1.05      2.5±0.30ms     6.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   275.5±18.21µs    10.7 MB/sec    1.03   285.0±17.52µs    10.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.0±0.16ms     5.1 MB/sec    1.03      5.1±0.18ms     5.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-03-24 23:20_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/assignment_default_in_stub.rs`:11 on 2023-03-24 23:20_

Do you plan to remove this? Or fill it in?

---

_@JonathanPlasse reviewed on 2023-03-24 23:23_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/flake8_pyi/rules/assignment_default_in_stub.rs`:11 on 2023-03-24 23:23_

This is just an example of what happens when adding a new rule.
All files in this PR except `scripts/add_rule.py` should be removed before merging.

---

_Renamed from "Improve add_rule.py to respect sorting order" to "Improve add_rule.py and add_plugin.py scripts" by @JonathanPlasse on 2023-03-25 10:39_

---

_Label `internal` added by @charliermarsh on 2023-03-25 15:58_

---

_Merged by @charliermarsh on 2023-03-25 16:05_

---

_Closed by @charliermarsh on 2023-03-25 16:05_

---

_Branch deleted on 2023-03-25 16:45_

---
