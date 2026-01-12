```yaml
number: 3590
title: Reduce usage of ALL in ecosystem CI
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/ecosystem-use-local-configuration
created_at: 2023-03-17T23:22:06Z
updated_at: 2023-03-18T22:31:55Z
url: https://github.com/astral-sh/ruff/pull/3590
synced_at: 2026-01-12T04:39:45Z
```

# Reduce usage of ALL in ecosystem CI

---

_Pull request opened by @charliermarsh on 2023-03-17 23:22_

## Summary

I want to enable us to add more repositories to the ecosystem CI. But if we run _all_ repositories with `--select ALL`, the check becomes more noisy (or, at least, repetitive, since every PR that adds a new check will show all occurrences in _every_ project). For now, I've left Bokeh, Zulip, and Airflow as `--select ALL`, and all new repositories can be added without an explicit select. This will allow us to catch breakages on these Ruff-using projects, while still benefiting from broad coverage via `--select ALL` on a few specific codebases.


---

_Review requested from @konstin by @charliermarsh on 2023-03-17 23:22_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-17 23:22_

---

_Label `internal` added by @charliermarsh on 2023-03-17 23:22_

---

_Comment by @github-actions[bot] on 2023-03-17 23:34_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.7±0.09ms     2.8 MB/sec    1.02     14.9±0.04ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.01ms     4.3 MB/sec    1.01      3.9±0.01ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    450.4±1.67µs     6.6 MB/sec    1.00    452.1±1.57µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.6±0.06ms     3.9 MB/sec    1.00      6.6±0.05ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.03ms     4.9 MB/sec    1.00      8.2±0.04ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1788.7±2.52µs     9.3 MB/sec    1.00   1779.4±5.98µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    185.7±0.26µs    15.9 MB/sec    1.01    186.8±0.55µs    15.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.01ms     6.7 MB/sec    1.00      3.8±0.01ms     6.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     12.9±0.05ms     3.2 MB/sec    1.00     12.9±0.03ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    373.4±1.71µs     7.9 MB/sec    1.00    373.4±2.49µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.6±0.02ms     4.6 MB/sec    1.00      5.6±0.06ms     4.6 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.01ms     5.5 MB/sec    1.00      7.4±0.02ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1535.1±4.21µs    10.8 MB/sec    1.00   1539.7±5.16µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    162.5±1.05µs    18.2 MB/sec    1.00    162.1±3.84µs    18.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.02ms     7.7 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-03-18 04:06_

---

_Review comment by @charliermarsh on `scripts/check_ecosystem.py`:64 on 2023-03-18 04:06_

Also adding `disnake`.

---

_@MichaReiser approved on 2023-03-18 08:32_

---

_Merged by @charliermarsh on 2023-03-18 17:13_

---

_Closed by @charliermarsh on 2023-03-18 17:13_

---

_Branch deleted on 2023-03-18 17:13_

---

_@henryiii reviewed on 2023-03-18 22:31_

---

_Review comment by @henryiii on `scripts/check_ecosystem.py`:64 on 2023-03-18 22:31_

This broke the check job entirely, as it's "master", not "main", in this check. :(

I think changes shouldn't fail jobs, but being completely broken should probably produce a red x on the job.

---
