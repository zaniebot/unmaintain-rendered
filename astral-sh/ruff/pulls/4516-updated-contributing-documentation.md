```yaml
number: 4516
title: Updated contributing documentation
type: pull_request
state: merged
author: hoel-bagard
labels:
  - documentation
assignees: []
merged: true
base: main
head: update_docs
created_at: 2023-05-19T03:45:38Z
updated_at: 2023-05-19T06:39:16Z
url: https://github.com/astral-sh/ruff/pull/4516
synced_at: 2026-01-12T03:50:03Z
```

# Updated contributing documentation

---

_Pull request opened by @hoel-bagard on 2023-05-19 03:45_

Fixed outdated path in the "adding a new lint rule" part of the contributing document.

---

_Comment by @github-actions[bot] on 2023-05-19 04:15_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.0±0.03ms     2.7 MB/sec    1.01     15.1±0.05ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.01ms     4.5 MB/sec    1.00      3.7±0.00ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    376.1±1.57µs     7.8 MB/sec    1.00    375.9±1.31µs     7.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.03ms     4.1 MB/sec    1.01      6.3±0.03ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.01ms     5.5 MB/sec    1.01      7.5±0.02ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1552.5±5.19µs    10.7 MB/sec    1.00   1560.1±5.66µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    168.6±0.41µs    17.5 MB/sec    1.01    170.3±1.24µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.7 MB/sec    1.01      3.4±0.01ms     7.6 MB/sec
parser/large/dataset.py                    1.00      6.0±0.01ms     6.7 MB/sec    1.01      6.1±0.03ms     6.7 MB/sec
parser/numpy/ctypeslib.py                  1.00   1181.4±0.89µs    14.1 MB/sec    1.01   1190.8±1.99µs    14.0 MB/sec
parser/numpy/globals.py                    1.00    122.7±0.24µs    24.1 MB/sec    1.00    123.2±0.54µs    24.0 MB/sec
parser/pydantic/types.py                   1.00      2.6±0.00ms    10.0 MB/sec    1.01      2.6±0.00ms     9.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.6±0.23ms     2.5 MB/sec    1.00     16.5±0.17ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     4.0 MB/sec    1.00      4.2±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    491.2±6.43µs     6.0 MB/sec    1.00    490.2±6.56µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.07ms     3.7 MB/sec    1.00      6.9±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.06ms     5.0 MB/sec    1.00      8.2±0.08ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1716.3±24.56µs     9.7 MB/sec    1.01  1729.9±24.52µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    192.5±3.44µs    15.3 MB/sec    1.01    195.4±8.87µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     7.0 MB/sec    1.00      3.7±0.04ms     7.0 MB/sec
parser/large/dataset.py                    1.10      7.3±0.07ms     5.5 MB/sec    1.00      6.7±0.07ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.08  1377.1±15.05µs    12.1 MB/sec    1.00  1273.1±20.55µs    13.1 MB/sec
parser/numpy/globals.py                    1.06    140.2±2.19µs    21.0 MB/sec    1.00    132.3±2.38µs    22.3 MB/sec
parser/pydantic/types.py                   1.08      3.1±0.03ms     8.3 MB/sec    1.00      2.8±0.03ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-05-19 06:38_

---

_Comment by @MichaReiser on 2023-05-19 06:39_

Thank you!

---

_Label `documentation` added by @MichaReiser on 2023-05-19 06:39_

---

_Merged by @MichaReiser on 2023-05-19 06:39_

---

_Closed by @MichaReiser on 2023-05-19 06:39_

---
