```yaml
number: 4503
title: Update RustPython dependencies
type: pull_request
state: merged
author: figsoda
labels: []
assignees: []
merged: true
base: main
head: update
created_at: 2023-05-18T19:04:49Z
updated_at: 2023-05-18T19:41:00Z
url: https://github.com/astral-sh/ruff/pull/4503
synced_at: 2026-01-12T03:50:03Z
```

# Update RustPython dependencies

---

_Pull request opened by @figsoda on 2023-05-18 19:04_

When trying to update ruff in nixpkgs, I ran into this error:

```
error: failed to load manifest for workspace member `/nix/store/mjg7kwf270mc18kld7a5l3f94vapjl2x-Parser-e820928/parser-pyo3`

Caused by:
  failed to read `<...>-Parser-e820928/parser-pyo3/Cargo.toml`

Caused by:
  No such file or directory (os error 2)
```

The error might be specific to nixpkgs, but we were on a broken commit of RustPython

mostly I'm just looking for https://github.com/RustPython/Parser/pull/45 which includes the fix

---

_Comment by @github-actions[bot] on 2023-05-18 19:16_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.0±0.02ms     2.9 MB/sec    1.00     13.9±0.02ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    423.7±0.59µs     7.0 MB/sec    1.00    422.6±1.50µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.8±0.09ms     4.4 MB/sec    1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.01      6.7±0.01ms     6.0 MB/sec    1.00      6.7±0.01ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1464.6±2.63µs    11.4 MB/sec    1.00   1452.7±1.47µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    165.1±3.36µs    17.9 MB/sec    1.00    163.1±1.51µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.00ms     8.4 MB/sec    1.00      3.0±0.00ms     8.4 MB/sec
parser/large/dataset.py                    1.00      5.3±0.00ms     7.6 MB/sec    1.00      5.3±0.00ms     7.6 MB/sec
parser/numpy/ctypeslib.py                  1.00   1054.7±1.14µs    15.8 MB/sec    1.00   1058.4±0.93µs    15.7 MB/sec
parser/numpy/globals.py                    1.00    108.2±0.21µs    27.3 MB/sec    1.02    110.1±6.82µs    26.8 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.1 MB/sec    1.00      2.3±0.00ms    11.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.4±0.19ms     2.3 MB/sec    1.00     17.3±0.14ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.4±0.05ms     3.8 MB/sec    1.00      4.3±0.05ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    515.5±9.86µs     5.7 MB/sec    1.00    510.6±6.45µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.3±0.08ms     3.5 MB/sec    1.00      7.2±0.10ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.02      8.6±0.07ms     4.8 MB/sec    1.00      8.4±0.06ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1793.3±20.25µs     9.3 MB/sec    1.00  1792.1±35.95µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    202.3±4.02µs    14.6 MB/sec    1.01    204.3±6.02µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.03ms     6.7 MB/sec    1.00      3.8±0.04ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.7±0.06ms     6.0 MB/sec    1.00      6.7±0.05ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.01  1286.0±26.33µs    12.9 MB/sec    1.00  1278.6±14.52µs    13.0 MB/sec
parser/numpy/globals.py                    1.00    132.2±2.51µs    22.3 MB/sec    1.00    132.1±1.92µs    22.3 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.03ms     8.9 MB/sec    1.00      2.9±0.03ms     8.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-05-18 19:28_

---

_Closed by @charliermarsh on 2023-05-18 19:28_

---

_Branch deleted on 2023-05-18 19:28_

---
