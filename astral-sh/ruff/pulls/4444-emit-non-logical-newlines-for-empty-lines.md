```yaml
number: 4444
title: "Emit non-logical newlines for \"empty\" lines"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/newlines
created_at: 2023-05-15T18:04:15Z
updated_at: 2023-05-16T15:18:21Z
url: https://github.com/astral-sh/ruff/pull/4444
synced_at: 2026-01-12T15:55:15Z
```

# Emit non-logical newlines for "empty" lines

---

_@charliermarsh_

## Summary

This PR pulls in https://github.com/RustPython/Parser/pull/27 and updates some of Ruff's internals to recognize the changes in the lexer.

Note that I'll merge https://github.com/RustPython/Parser/pull/27 and update the `Cargo.toml` here prior to merging this change.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-15 18:04_

---

_Comment by @github-actions[bot] on 2023-05-15 18:45_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.3±0.07ms     2.4 MB/sec    1.01     17.5±0.10ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     3.9 MB/sec    1.00      4.2±0.04ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.07   547.9±41.24µs     5.4 MB/sec    1.00    512.7±1.47µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.06ms     3.5 MB/sec    1.00      7.2±0.05ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.04ms     4.9 MB/sec    1.01      8.4±0.05ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1764.3±3.10µs     9.4 MB/sec    1.01   1777.7±5.73µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.02   202.1±10.57µs    14.6 MB/sec    1.00    199.1±0.34µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.02ms     6.9 MB/sec    1.01      3.7±0.02ms     6.9 MB/sec
parser/large/dataset.py                    1.01      6.5±0.01ms     6.2 MB/sec    1.00      6.5±0.01ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.01   1281.6±1.36µs    13.0 MB/sec    1.00   1274.4±1.35µs    13.1 MB/sec
parser/numpy/globals.py                    1.00    133.0±0.35µs    22.2 MB/sec    1.00    132.6±0.30µs    22.2 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.01ms     9.2 MB/sec    1.00      2.8±0.01ms     9.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.5±0.14ms     2.5 MB/sec    1.00     16.5±0.16ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.06ms     3.9 MB/sec    1.00      4.2±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01   505.3±17.28µs     5.8 MB/sec    1.00    499.5±6.15µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.09ms     3.7 MB/sec    1.01      7.1±0.09ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.06ms     5.0 MB/sec    1.00      8.2±0.07ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1740.5±20.52µs     9.6 MB/sec    1.00  1726.2±20.15µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    196.7±3.69µs    15.0 MB/sec    1.01    198.0±6.75µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.05ms     6.9 MB/sec    1.00      3.7±0.05ms     7.0 MB/sec
parser/large/dataset.py                    1.00      6.7±0.05ms     6.1 MB/sec    1.01      6.8±0.05ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1272.6±18.76µs    13.1 MB/sec    1.01  1291.6±18.07µs    12.9 MB/sec
parser/numpy/globals.py                    1.00    131.1±2.13µs    22.5 MB/sec    1.00    131.1±2.01µs    22.5 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.0 MB/sec    1.01      2.8±0.05ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/doc_lines.rs`:16 on 2023-05-16 06:29_

Nice, no dependency on the locator :)

---

_@MichaReiser approved on 2023-05-16 06:32_

---

_Merged by @charliermarsh on 2023-05-16 14:58_

---

_Closed by @charliermarsh on 2023-05-16 14:58_

---

_Branch deleted on 2023-05-16 14:58_

---
