```yaml
number: 4035
title: Support --fix in watch mode
type: pull_request
state: merged
author: evanrittenhouse
labels: []
assignees: []
merged: true
base: main
head: 1064_fix_in_watch
created_at: 2023-04-20T01:01:27Z
updated_at: 2023-06-15T21:15:55Z
url: https://github.com/astral-sh/ruff/pull/4035
synced_at: 2026-01-12T03:43:29Z
```

# Support --fix in watch mode

---

_Pull request opened by @evanrittenhouse on 2023-04-20 01:01_

Fixes #1064

Manual test:
1. Create empty `test.py` file
2. run `cargo -p run ruff_cli -- check test.py --fix --watch`
3. Change test.py to:
```
x = (1)
```
 and save

Expected output:
`test.py` should be:
```
x = 1
```

---

_Comment by @github-actions[bot] on 2023-04-20 01:12_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.2±0.04ms     2.9 MB/sec    1.00     14.2±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.00      3.6±0.02ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    457.4±1.19µs     6.5 MB/sec    1.00    458.3±1.29µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.05ms     4.2 MB/sec    1.01      6.1±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.01ms     5.7 MB/sec    1.01      7.2±0.02ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1619.5±1.86µs    10.3 MB/sec    1.02   1646.7±4.14µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    181.2±0.42µs    16.3 MB/sec    1.00    181.7±0.57µs    16.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.7 MB/sec    1.02      3.4±0.01ms     7.6 MB/sec
parser/large/dataset.py                    1.00      5.7±0.00ms     7.1 MB/sec  
parser/numpy/ctypeslib.py                  1.00   1145.6±0.52µs    14.5 MB/sec  
parser/numpy/globals.py                    1.00    114.7±0.26µs    25.7 MB/sec  
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.3 MB/sec  
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     16.7±0.18ms     2.4 MB/sec    1.00     16.4±0.16ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.3±0.08ms     3.9 MB/sec    1.00      4.2±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.02    508.1±7.62µs     5.8 MB/sec    1.00    499.4±7.67µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.1±0.13ms     3.6 MB/sec    1.00      7.0±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.4±0.08ms     4.8 MB/sec    1.00      8.3±0.06ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1828.7±24.76µs     9.1 MB/sec    1.01  1841.2±26.41µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    196.5±3.67µs    15.0 MB/sec    1.00    196.5±5.12µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.12ms     6.7 MB/sec    1.01      3.8±0.05ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.6±0.05ms     6.2 MB/sec  
parser/numpy/ctypeslib.py                  1.00  1257.8±18.07µs    13.2 MB/sec  
parser/numpy/globals.py                    1.00    126.5±3.30µs    23.3 MB/sec  
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.0 MB/sec  
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @evanrittenhouse on 2023-04-20 01:14_

---

_Merged by @charliermarsh on 2023-04-20 03:33_

---

_Closed by @charliermarsh on 2023-04-20 03:33_

---

_Comment by @charliermarsh on 2023-04-20 03:33_

Thanks!

---

_Branch deleted on 2023-06-15 21:15_

---
