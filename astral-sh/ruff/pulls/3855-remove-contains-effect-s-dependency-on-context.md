```yaml
number: 3855
title: "Remove `contains_effect`'s dependency on `Context`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/contains-effect
created_at: 2023-04-02T22:22:30Z
updated_at: 2023-04-03T16:17:05Z
url: https://github.com/astral-sh/ruff/pull/3855
synced_at: 2026-01-12T04:28:19Z
```

# Remove `contains_effect`'s dependency on `Context`

---

_Pull request opened by @charliermarsh on 2023-04-02 22:22_

_No description provided._

---

_Renamed from "Remove contains_effect's dependency on Context" to "Remove `contains_effect`'s dependency on `Context`" by @charliermarsh on 2023-04-02 22:22_

---

_Comment by @github-actions[bot] on 2023-04-02 22:34_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+8, -0, 0 error(s))

<details><summary>bokeh (+8, -0)</summary>
<p>

```diff
+ tests/unit/bokeh/_testing/util/test_env.py:1:1: D100 Missing docstring in public module
+ tests/unit/bokeh/_testing/util/test_env.py:45:5: D103 Missing docstring in public function
+ tests/unit/bokeh/_testing/util/test_env.py:51:5: D103 Missing docstring in public function
+ tests/unit/bokeh/_testing/util/test_env.py:66:5: D103 Missing docstring in public function
+ tests/unit/bokeh/_testing/util/test_env.py:73:5: D103 Missing docstring in public function
+ tests/unit/bokeh/_testing/util/test_env.py:80:5: D103 Missing docstring in public function
+ tests/unit/bokeh/_testing/util/test_env.py:87:5: D103 Missing docstring in public function
+ tests/unit/bokeh/_testing/util/test_env.py:94:5: D103 Missing docstring in public function
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.3±0.06ms     2.7 MB/sec    1.00     15.3±0.08ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.9±0.02ms     4.3 MB/sec    1.00      3.8±0.02ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.01    413.4±1.51µs     7.1 MB/sec    1.00    409.8±2.10µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.6±0.04ms     3.9 MB/sec    1.00      6.5±0.03ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.01      8.0±0.03ms     5.1 MB/sec    1.00      7.9±0.03ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1756.8±3.63µs     9.5 MB/sec    1.00   1739.4±5.30µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    183.1±0.36µs    16.1 MB/sec    1.00    182.8±1.24µs    16.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.01ms     6.9 MB/sec    1.00      3.6±0.00ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.2±0.99ms     2.4 MB/sec    1.07     18.4±1.01ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.05      4.6±0.30ms     3.6 MB/sec    1.00      4.4±0.21ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.11   554.9±35.47µs     5.3 MB/sec    1.00   500.5±14.65µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.05      7.6±0.49ms     3.4 MB/sec    1.00      7.3±0.31ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.8±0.31ms     4.6 MB/sec    1.01      8.9±0.57ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1980.1±61.51µs     8.4 MB/sec    1.02      2.0±0.09ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    198.7±6.39µs    14.8 MB/sec    1.09   217.4±15.51µs    13.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.16ms     6.2 MB/sec    1.01      4.2±0.18ms     6.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @charliermarsh on 2023-04-03 15:52_

---

_Merged by @charliermarsh on 2023-04-03 16:08_

---

_Closed by @charliermarsh on 2023-04-03 16:08_

---

_Branch deleted on 2023-04-03 16:08_

---
