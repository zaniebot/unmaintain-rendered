```yaml
number: 4081
title: "Ignore `ClassVar` annotation for `RUF008`, `RUF009`"
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: main
head: fix/ignore-classvar
created_at: 2023-04-24T15:48:13Z
updated_at: 2023-04-25T12:07:49Z
url: https://github.com/astral-sh/ruff/pull/4081
synced_at: 2026-01-12T04:28:19Z
```

# Ignore `ClassVar` annotation for `RUF008`, `RUF009`

---

_Pull request opened by @dhruvmanila on 2023-04-24 15:48_

fixes: #4077

---

_Comment by @github-actions[bot] on 2023-04-24 15:59_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     20.0±0.74ms     2.0 MB/sec    1.00     19.8±0.77ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.7±0.21ms     3.5 MB/sec    1.01      4.8±0.18ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   606.4±20.47µs     4.9 MB/sec    1.00   608.8±27.58µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.4±0.29ms     3.1 MB/sec    1.00      8.4±0.23ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.07      9.8±0.37ms     4.1 MB/sec    1.00      9.2±0.22ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03      2.1±0.08ms     7.9 MB/sec    1.00      2.0±0.04ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.03    252.0±7.41µs    11.7 MB/sec    1.00   244.1±12.81µs    12.1 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.3±0.14ms     5.9 MB/sec    1.00      4.2±0.11ms     6.0 MB/sec
parser/large/dataset.py                    1.00      7.6±0.17ms     5.4 MB/sec    1.03      7.8±0.19ms     5.2 MB/sec
parser/numpy/ctypeslib.py                  1.00  1520.6±48.77µs    11.0 MB/sec    1.04  1576.2±97.17µs    10.6 MB/sec
parser/numpy/globals.py                    1.02   160.0±10.77µs    18.4 MB/sec    1.00    156.6±4.96µs    18.8 MB/sec
parser/pydantic/types.py                   1.00      3.3±0.09ms     7.6 MB/sec    1.00      3.3±0.09ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.7±0.10ms     2.4 MB/sec    1.00     16.5±0.09ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.05ms     3.8 MB/sec    1.00      4.3±0.05ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    539.2±8.28µs     5.5 MB/sec    1.00    537.9±7.09µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.1±0.11ms     3.6 MB/sec    1.00      7.1±0.07ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.02      8.7±0.05ms     4.7 MB/sec    1.00      8.5±0.04ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1907.9±25.49µs     8.7 MB/sec    1.00  1883.6±31.51µs     8.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    216.8±5.40µs    13.6 MB/sec    1.00    215.9±7.94µs    13.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.03ms     6.5 MB/sec    1.00      3.9±0.05ms     6.5 MB/sec
parser/large/dataset.py                    1.01      6.9±0.04ms     5.9 MB/sec    1.00      6.8±0.05ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.01  1302.5±13.26µs    12.8 MB/sec    1.00  1288.6±11.59µs    12.9 MB/sec
parser/numpy/globals.py                    1.02    132.0±1.76µs    22.4 MB/sec    1.00    130.0±1.63µs    22.7 MB/sec
parser/pydantic/types.py                   1.01      2.9±0.02ms     8.7 MB/sec    1.00      2.9±0.02ms     8.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @JonathanPlasse on 2023-04-24 19:03_

Does `typing.Final` work correctly?

---

_Merged by @charliermarsh on 2023-04-24 23:58_

---

_Closed by @charliermarsh on 2023-04-24 23:58_

---

_Comment by @dhruvmanila on 2023-04-25 01:24_

> Does `typing.Final` work correctly?

I don't think that's relevant here. Can you give me an example related to that?

---

_Branch deleted on 2023-04-25 01:24_

---

_Comment by @JonathanPlasse on 2023-04-25 12:07_

Yes, you are right.

---
