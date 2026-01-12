```yaml
number: 4253
title: Consider Flask app logger as logger candidate
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
assignees: []
merged: true
base: main
head: fix/flask-logger-candidate
created_at: 2023-05-06T07:22:09Z
updated_at: 2023-05-13T16:38:58Z
url: https://github.com/astral-sh/ruff/pull/4253
synced_at: 2026-01-12T03:50:02Z
```

# Consider Flask app logger as logger candidate

---

_Pull request opened by @dhruvmanila on 2023-05-06 07:22_

For future reference, the logger can also be invoked directly through the `app` variable as mentioned below. But, this requires type inference.

```python
from flask import Flask

app = Flask(__name__)
app.logger.info("...")
```

fixes: #4251

---

_Comment by @github-actions[bot] on 2023-05-06 07:33_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.0±0.04ms     2.9 MB/sec    1.01     14.1±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.02ms     4.9 MB/sec    1.00      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    423.5±0.61µs     7.0 MB/sec    1.00    422.3±0.66µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.01      5.9±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.01ms     5.9 MB/sec    1.02      7.0±0.02ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1489.6±2.56µs    11.2 MB/sec    1.01   1511.4±3.22µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    167.6±0.27µs    17.6 MB/sec    1.01    168.8±1.44µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.2 MB/sec    1.02      3.1±0.02ms     8.1 MB/sec
parser/large/dataset.py                    1.01      5.4±0.01ms     7.5 MB/sec    1.00      5.4±0.00ms     7.5 MB/sec
parser/numpy/ctypeslib.py                  1.01   1058.2±1.28µs    15.7 MB/sec    1.00   1048.3±1.52µs    15.9 MB/sec
parser/numpy/globals.py                    1.01    107.8±0.42µs    27.4 MB/sec    1.00    106.9±0.16µs    27.6 MB/sec
parser/pydantic/types.py                   1.01      2.3±0.00ms    11.0 MB/sec    1.00      2.3±0.00ms    11.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     18.9±0.63ms     2.2 MB/sec    1.04     19.7±0.64ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.05      5.2±0.42ms     3.2 MB/sec    1.00      5.0±0.17ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.04   609.6±25.84µs     4.8 MB/sec    1.00   585.4±21.48µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.02      8.3±0.37ms     3.1 MB/sec    1.00      8.2±0.34ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.06     10.4±1.12ms     3.9 MB/sec    1.00      9.9±0.28ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.09      2.3±0.17ms     7.3 MB/sec    1.00      2.1±0.09ms     8.0 MB/sec
linter/default-rules/numpy/globals.py      1.00   244.3±17.22µs    12.1 MB/sec    1.04   253.4±15.45µs    11.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.24ms     6.0 MB/sec    1.06      4.5±0.34ms     5.6 MB/sec
parser/large/dataset.py                    1.00      7.9±0.23ms     5.1 MB/sec    1.00      8.0±0.23ms     5.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1502.1±61.61µs    11.1 MB/sec    1.05  1573.2±66.40µs    10.6 MB/sec
parser/numpy/globals.py                    1.00    151.4±5.54µs    19.5 MB/sec    1.04    157.4±6.70µs    18.8 MB/sec
parser/pydantic/types.py                   1.00      3.4±0.11ms     7.6 MB/sec    1.02      3.4±0.13ms     7.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-05-06 15:31_

---

_Closed by @charliermarsh on 2023-05-06 15:31_

---

_Label `bug` added by @charliermarsh on 2023-05-06 15:31_

---

_Branch deleted on 2023-05-13 16:38_

---
