```yaml
number: 4574
title: Add a script to update the schemastore
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: add_upate_schemastore_py
created_at: 2023-05-22T11:02:45Z
updated_at: 2023-05-23T11:01:37Z
url: https://github.com/astral-sh/ruff/pull/4574
synced_at: 2026-01-12T15:55:15Z
```

# Add a script to update the schemastore

---

_@konstin_

Hacked this together, it clones astral-sh/schemastore, updated the schema and pushes the changes to a new branch tagged with the ruff git hash. You can see the URL to create the PR to schemastore in the CLI. The script is separated into three blocks so you can rerun the schema generation in the middle before committing.

https://github.com/SchemaStore/schemastore/pull/2974 was created with this script

---

_Review requested from @charliermarsh by @konstin on 2023-05-22 11:02_

---

_Comment by @github-actions[bot] on 2023-05-22 11:14_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.0±0.15ms     2.4 MB/sec    1.00     16.9±0.14ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.0±0.04ms     4.1 MB/sec    1.00      4.0±0.03ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    495.0±2.86µs     6.0 MB/sec    1.00    493.4±2.42µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.06ms     3.7 MB/sec    1.00      6.9±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      7.9±0.04ms     5.1 MB/sec    1.00      7.8±0.08ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1716.7±6.46µs     9.7 MB/sec    1.00  1702.0±12.90µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.01    187.4±1.94µs    15.7 MB/sec    1.00    185.7±3.41µs    15.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.01ms     7.1 MB/sec    1.00      3.5±0.04ms     7.2 MB/sec
parser/large/dataset.py                    1.01      6.2±0.01ms     6.6 MB/sec    1.00      6.1±0.08ms     6.6 MB/sec
parser/numpy/ctypeslib.py                  1.00   1218.0±5.47µs    13.7 MB/sec    1.00  1216.8±12.09µs    13.7 MB/sec
parser/numpy/globals.py                    1.00    126.1±0.36µs    23.4 MB/sec    1.00    126.5±2.23µs    23.3 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.01ms     9.6 MB/sec    1.00      2.7±0.02ms     9.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.5±0.12ms     2.5 MB/sec    1.00     16.5±0.13ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     4.0 MB/sec    1.00      4.1±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    496.0±8.46µs     5.9 MB/sec    1.00    493.6±8.63µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.07ms     3.7 MB/sec    1.00      6.9±0.08ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.07ms     5.0 MB/sec    1.00      8.1±0.05ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1686.3±18.25µs     9.9 MB/sec    1.00  1694.2±18.96µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    189.9±3.85µs    15.5 MB/sec    1.01    191.8±5.08µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.0 MB/sec    1.00      3.6±0.04ms     7.0 MB/sec
parser/large/dataset.py                    1.00      6.5±0.04ms     6.2 MB/sec    1.00      6.5±0.04ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.00  1237.3±16.98µs    13.5 MB/sec    1.00  1235.3±12.84µs    13.5 MB/sec
parser/numpy/globals.py                    1.00    125.6±1.79µs    23.5 MB/sec    1.01    126.4±2.46µs    23.3 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.2 MB/sec    1.00      2.8±0.03ms     9.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-05-22 14:09_

---

_Review comment by @charliermarsh on `scripts/update_schemastore.py`:46 on 2023-05-22 14:09_

Are these intentional?

---

_@charliermarsh reviewed on 2023-05-22 14:09_

---

_Review comment by @charliermarsh on `scripts/update_schemastore.py`:33 on 2023-05-22 14:09_

I'm tempted to say we should just use a temporary directory for SchemaStore, but is that more trouble than it's worth?

---

_@charliermarsh approved on 2023-05-22 14:41_

---

_@konstin reviewed on 2023-05-23 10:05_

---

_Review comment by @konstin on `scripts/update_schemastore.py`:46 on 2023-05-23 10:05_

if you enable scientific mode you can run each block individually, like below, but i've removed it for the tempdir

![image](https://github.com/charliermarsh/ruff/assets/6826232/84665ace-3164-4fd0-97a5-cc75df188044)


---

_@konstin reviewed on 2023-05-23 10:06_

---

_Review comment by @konstin on `scripts/update_schemastore.py`:33 on 2023-05-23 10:06_

added the default to tempdir

---

_Merged by @konstin on 2023-05-23 10:41_

---

_Closed by @konstin on 2023-05-23 10:41_

---

_Branch deleted on 2023-05-23 10:41_

---
