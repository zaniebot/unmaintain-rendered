```yaml
number: 4021
title: Implement rule for WPS111
type: pull_request
state: closed
author: kytta
labels: []
assignees: []
draft: true
base: main
head: wps111-too_short_name
created_at: 2023-04-19T14:52:34Z
updated_at: 2023-10-19T16:12:23Z
url: https://github.com/astral-sh/ruff/pull/4021
synced_at: 2026-01-12T15:55:14Z
```

# Implement rule for WPS111

---

_@kytta_

This is one of the many rules one would need to implement for #3845. This PR implements WPS11, which checks that the names are not too short.

- [ ] modules (not sure about that one. Filesystem checker?)
- [x] variables
- [ ] attributes
- [ ] functions
- [ ] methods
- [ ] classes

Being new to the project (and Rust in general), I _really_ hope I am doing this right :D

---

_Comment by @github-actions[bot] on 2023-04-19 15:24_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.04     18.1±0.27ms     2.3 MB/sec    1.00     17.4±0.08ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.5±0.08ms     3.7 MB/sec    1.00      4.4±0.12ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.04   573.1±13.74µs     5.1 MB/sec    1.00    552.7±5.06µs     5.3 MB/sec
linter/all-rules/pydantic/types.py         1.04      7.7±0.15ms     3.3 MB/sec    1.00      7.4±0.05ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.04      9.0±0.13ms     4.5 MB/sec    1.00      8.7±0.05ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04      2.0±0.06ms     8.2 MB/sec    1.00   1952.1±5.63µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    214.1±6.03µs    13.8 MB/sec    1.05    224.9±7.44µs    13.1 MB/sec
linter/default-rules/pydantic/types.py     1.04      4.2±0.12ms     6.1 MB/sec    1.00      4.0±0.02ms     6.4 MB/sec
parser/large/dataset.py                    1.04      7.2±0.17ms     5.6 MB/sec    1.00      6.9±0.02ms     5.9 MB/sec
parser/numpy/ctypeslib.py                  1.05  1436.0±52.44µs    11.6 MB/sec    1.00   1368.7±1.95µs    12.2 MB/sec
parser/numpy/globals.py                    1.06    142.4±5.91µs    20.7 MB/sec    1.00    134.5±0.45µs    21.9 MB/sec
parser/pydantic/types.py                   1.05      3.1±0.06ms     8.2 MB/sec    1.00      3.0±0.00ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.8±0.04ms     2.6 MB/sec    1.01     16.0±0.04ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.01ms     4.0 MB/sec    1.00      4.2±0.02ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    434.5±4.88µs     6.8 MB/sec    1.01    438.1±6.22µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.05ms     3.7 MB/sec    1.00      6.9±0.02ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.06ms     5.0 MB/sec    1.00      8.2±0.02ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1762.2±8.77µs     9.4 MB/sec    1.01  1782.5±27.96µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    185.8±1.71µs    15.9 MB/sec    1.01    188.2±6.30µs    15.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.02ms     6.8 MB/sec    1.00      3.7±0.03ms     6.8 MB/sec
parser/large/dataset.py                    1.01      6.6±0.02ms     6.1 MB/sec    1.00      6.6±0.01ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.02   1276.0±6.95µs    13.0 MB/sec    1.00   1252.1±5.35µs    13.3 MB/sec
parser/numpy/globals.py                    1.01    128.2±1.07µs    23.0 MB/sec    1.00    126.8±1.75µs    23.3 MB/sec
parser/pydantic/types.py                   1.01      2.8±0.01ms     9.0 MB/sec    1.00      2.8±0.02ms     9.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @zanieb on 2023-10-19 16:12_

Thanks for taking the time to contribute! It looks like this was extended to be complete in https://github.com/astral-sh/ruff/pull/4021 and I'm going to close this in favor of that for book keeping purposes.

---

_Closed by @zanieb on 2023-10-19 16:12_

---
