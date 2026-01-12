```yaml
number: 4148
title: Move typos to pre-commit config
type: pull_request
state: merged
author: calumy
labels: []
assignees: []
merged: true
base: main
head: move-typos
created_at: 2023-04-29T10:08:29Z
updated_at: 2023-05-10T14:55:05Z
url: https://github.com/astral-sh/ruff/pull/4148
synced_at: 2026-01-12T15:55:14Z
```

# Move typos to pre-commit config

---

_@calumy_

For users of pre-commit, it would be useful to add the typos pre-commit check that currently runs in CI. With this proposed change, CI would run the typos check twice (in the pre-commit stage and its own stage), which is not necessary.

Moving typos to pre-commit also identified a couple of typos that were not picked up with the original CI version of the tool. One that was fixed in blacks test cases in https://github.com/psf/black/pull/3599 and one that is [not considered a typo in black](https://github.com/psf/black/blob/main/tests/data/simple_cases/docstring.py#L47) that has been added to `_typos.toml`.

---

_Comment by @github-actions[bot] on 2023-04-29 10:18_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.1±0.07ms     2.9 MB/sec    1.01     14.3±0.12ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.01      3.4±0.03ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    421.4±0.81µs     7.0 MB/sec    1.01    423.5±1.29µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.04ms     4.3 MB/sec    1.02      6.0±0.06ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.02ms     5.9 MB/sec    1.01      7.0±0.03ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1489.3±3.04µs    11.2 MB/sec    1.01   1508.2±4.50µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    168.7±0.30µs    17.5 MB/sec    1.00    169.0±0.83µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.2 MB/sec    1.02      3.2±0.04ms     8.1 MB/sec
parser/large/dataset.py                    1.01      5.5±0.01ms     7.3 MB/sec    1.00      5.5±0.06ms     7.4 MB/sec
parser/numpy/ctypeslib.py                  1.01   1078.3±1.39µs    15.4 MB/sec    1.00   1063.3±0.77µs    15.7 MB/sec
parser/numpy/globals.py                    1.01    109.5±0.20µs    27.0 MB/sec    1.00    108.7±1.16µs    27.2 MB/sec
parser/pydantic/types.py                   1.01      2.3±0.00ms    10.9 MB/sec    1.00      2.3±0.00ms    11.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     20.5±0.78ms  2027.3 KB/sec    1.03     21.2±0.71ms  1963.3 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      5.3±0.40ms     3.1 MB/sec    1.00      5.2±0.28ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.04   624.2±29.24µs     4.7 MB/sec    1.00   597.6±34.74µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.8±0.45ms     2.9 MB/sec    1.01      8.9±0.44ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.01     10.6±0.45ms     3.8 MB/sec    1.00     10.5±0.44ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.13ms     7.6 MB/sec    1.00      2.2±0.11ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   256.7±16.46µs    11.5 MB/sec    1.03   263.9±24.98µs    11.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.8±0.31ms     5.3 MB/sec    1.00      4.8±0.27ms     5.3 MB/sec
parser/large/dataset.py                    1.00      8.4±0.29ms     4.9 MB/sec    1.02      8.5±0.28ms     4.8 MB/sec
parser/numpy/ctypeslib.py                  1.00  1631.0±61.85µs    10.2 MB/sec    1.01  1643.3±78.86µs    10.1 MB/sec
parser/numpy/globals.py                    1.00   164.7±11.40µs    17.9 MB/sec    1.00   165.4±10.11µs    17.8 MB/sec
parser/pydantic/types.py                   1.00      3.6±0.13ms     7.0 MB/sec    1.00      3.6±0.17ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-04-29 16:13_

---

_Closed by @charliermarsh on 2023-04-29 16:13_

---

_Comment by @charliermarsh on 2023-04-29 16:13_

Thanks! Any idea why those weren't being identified before?

---

_Comment by @calumy on 2023-04-29 19:49_

> Thanks! Any idea why those weren't being identified before?

I'm not sure I'm afraid. I have only used the pre-commit version before. 

---

_Branch deleted on 2023-05-10 14:55_

---
