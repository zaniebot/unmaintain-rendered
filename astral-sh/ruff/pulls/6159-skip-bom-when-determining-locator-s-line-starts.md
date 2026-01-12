```yaml
number: 6159
title: "Skip BOM when determining Locator's line starts"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/bom
created_at: 2023-07-28T17:52:00Z
updated_at: 2023-07-29T12:07:26Z
url: https://github.com/astral-sh/ruff/pull/6159
synced_at: 2026-01-12T15:55:20Z
```

# Skip BOM when determining Locator's line starts

---

_@charliermarsh_

## Summary

If a file has a BOM, the import sorter _always_ reports the imports as unsorted. The acute issue is that we detect that the line has leading content (before the imports), which we always consider a violation. Rather than fixing that one site, this PR instead makes `.line_start` BOM-aware.

Fixes https://github.com/astral-sh/ruff/issues/6155.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-28 17:52_

---

_Comment by @github-actions[bot] on 2023-07-28 18:05_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.6±0.23ms     4.7 MB/sec    1.01      8.7±0.28ms     4.7 MB/sec
formatter/numpy/ctypeslib.py               1.00  1657.2±19.47µs    10.0 MB/sec    1.01  1667.8±38.04µs    10.0 MB/sec
formatter/numpy/globals.py                 1.00    181.1±4.04µs    16.3 MB/sec    1.00    180.6±3.84µs    16.3 MB/sec
formatter/pydantic/types.py                1.00      3.6±0.04ms     7.2 MB/sec    1.00      3.6±0.04ms     7.2 MB/sec
linter/all-rules/large/dataset.py          1.01     11.2±0.12ms     3.6 MB/sec    1.00     11.1±0.11ms     3.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.01ms     5.9 MB/sec    1.01      2.8±0.05ms     5.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    383.3±2.60µs     7.7 MB/sec    1.00    383.2±1.07µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.0±0.10ms     5.1 MB/sec    1.00      5.0±0.18ms     5.1 MB/sec
linter/default-rules/large/dataset.py      1.01      6.0±0.07ms     6.8 MB/sec    1.00      5.9±0.14ms     6.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1247.9±20.50µs    13.3 MB/sec    1.00   1238.8±6.76µs    13.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    134.3±0.25µs    22.0 MB/sec    1.00    133.7±0.39µs    22.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.7±0.04ms     9.6 MB/sec    1.00      2.6±0.05ms     9.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.3±0.25ms     4.0 MB/sec    1.05     10.8±0.14ms     3.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1964.1±19.12µs     8.5 MB/sec    1.05      2.1±0.03ms     8.0 MB/sec
formatter/numpy/globals.py                 1.00    217.1±5.21µs    13.6 MB/sec    1.05    227.0±6.44µs    13.0 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.06ms     5.9 MB/sec    1.06      4.5±0.06ms     5.6 MB/sec
linter/all-rules/large/dataset.py          1.02     13.9±0.25ms     2.9 MB/sec    1.00     13.6±0.13ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.6±0.04ms     4.6 MB/sec    1.00      3.6±0.06ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    439.1±6.34µs     6.7 MB/sec    1.00    433.7±9.62µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.03      6.3±0.09ms     4.1 MB/sec    1.00      6.1±0.07ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.6±0.08ms     5.3 MB/sec    1.00      7.5±0.07ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1543.6±22.75µs    10.8 MB/sec    1.00  1521.3±21.45µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.02    171.5±3.12µs    17.2 MB/sec    1.00    168.3±2.54µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.3±0.04ms     7.7 MB/sec    1.00      3.3±0.04ms     7.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/isort/rules/organize_imports.rs`:64 on 2023-07-29 07:57_

Nit: Revert this change to an, otherwise, unchanged file?

---

_@MichaReiser approved on 2023-07-29 08:01_

---

_Merged by @charliermarsh on 2023-07-29 11:47_

---

_Closed by @charliermarsh on 2023-07-29 11:47_

---

_Branch deleted on 2023-07-29 11:47_

---
