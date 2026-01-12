```yaml
number: 5560
title: "Add httpx into the `Who's Using Ruff?` section"
type: pull_request
state: merged
author: karpetrosyan
labels:
  - documentation
assignees: []
merged: true
base: main
head: add-httpx-into-readme
created_at: 2023-07-06T12:47:17Z
updated_at: 2023-07-06T14:07:54Z
url: https://github.com/astral-sh/ruff/pull/5560
synced_at: 2026-01-12T03:36:55Z
```

# Add httpx into the `Who's Using Ruff?` section

---

_Pull request opened by @karpetrosyan on 2023-07-06 12:47_

_No description provided._

---

_Comment by @github-actions[bot] on 2023-07-06 13:12_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.8±0.04ms     5.2 MB/sec    1.00      7.7±0.04ms     5.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1716.1±2.50µs     9.7 MB/sec    1.00   1713.7±5.43µs     9.7 MB/sec
formatter/numpy/globals.py                 1.00    191.3±0.45µs    15.4 MB/sec    1.00    190.9±1.56µs    15.5 MB/sec
formatter/pydantic/types.py                1.01      3.8±0.01ms     6.8 MB/sec    1.00      3.7±0.01ms     6.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.3±0.11ms     3.1 MB/sec    1.02     13.6±0.29ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.03ms     5.0 MB/sec    1.00      3.3±0.03ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    422.4±0.94µs     7.0 MB/sec    1.00    421.8±2.90µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.02      5.9±0.08ms     4.3 MB/sec    1.00      5.8±0.04ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.04ms     6.2 MB/sec    1.00      6.6±0.03ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1420.8±3.11µs    11.7 MB/sec    1.00   1414.8±3.24µs    11.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    159.1±0.25µs    18.5 MB/sec    1.01    160.3±0.48µs    18.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.6 MB/sec    1.00      3.0±0.01ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.2±0.36ms     3.6 MB/sec    1.01     11.3±0.27ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.4±0.10ms     6.9 MB/sec    1.02      2.5±0.09ms     6.8 MB/sec
formatter/numpy/globals.py                 1.00   279.7±10.03µs    10.5 MB/sec    1.01   283.9±20.15µs    10.4 MB/sec
formatter/pydantic/types.py                1.00      5.3±0.19ms     4.8 MB/sec    1.02      5.4±0.23ms     4.7 MB/sec
linter/all-rules/large/dataset.py          1.08    22.9±10.84ms  1822.3 KB/sec    1.00     21.1±0.48ms  1973.3 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.0±0.16ms     3.3 MB/sec    1.07      5.3±0.17ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   621.7±19.81µs     4.7 MB/sec    1.04   643.6±20.40µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.1±1.03ms     2.8 MB/sec    1.02      9.3±0.30ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.00      9.9±0.25ms     4.1 MB/sec    1.18     11.7±0.28ms     3.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.06ms     8.2 MB/sec    1.16      2.4±0.06ms     7.0 MB/sec
linter/default-rules/numpy/globals.py      1.00   247.4±10.67µs    11.9 MB/sec    1.10   272.3±10.22µs    10.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.14ms     5.9 MB/sec    1.18      5.1±0.13ms     5.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@zanieb reviewed on 2023-07-06 13:18_

---

_Review comment by @zanieb on `README.md`:363 on 2023-07-06 13:18_

In the README the name is stylized as such:

```suggestion
- [HTTPX](https://github.com/encode/httpx)
```

---

_Comment by @zanieb on 2023-07-06 13:18_

Thanks for the contribution!

xref https://github.com/encode/httpx/pull/2648

---

_Comment by @charliermarsh on 2023-07-06 13:20_

(The MkDocs failure is my bad — need to change that step to only do the Insiders install if the secret is present.)

---

_@zanieb approved on 2023-07-06 13:23_

---

_Label `documentation` added by @charliermarsh on 2023-07-06 13:46_

---

_Merged by @charliermarsh on 2023-07-06 13:52_

---

_Closed by @charliermarsh on 2023-07-06 13:52_

---
