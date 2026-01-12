```yaml
number: 6463
title: "Document default behavior of `W505` in setting"
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/W505
created_at: 2023-08-09T21:32:14Z
updated_at: 2023-08-11T21:41:33Z
url: https://github.com/astral-sh/ruff/pull/6463
synced_at: 2026-01-12T02:52:04Z
```

# Document default behavior of `W505` in setting

---

_Pull request opened by @zanieb on 2023-08-09 21:32_

Addresses https://github.com/astral-sh/ruff/discussions/6459

---

_Renamed from "Document default behavior of `W505`" to "Document default behavior of `W505` in setting" by @zanieb on 2023-08-09 21:33_

---

_@jamesbraza reviewed on 2023-08-09 21:52_

---

_Review comment by @jamesbraza on `crates/ruff/src/rules/pycodestyle/settings.rs`:24 on 2023-08-09 21:52_

```suggestion
    /// Defaults to `None`, which disables this rule (default does not match `line-length`).  See [`doc-line-too-long`](https://beta.ruff.rs/docs/rules/doc-line-too-long/) for more information.
```

---

_Comment by @github-actions[bot] on 2023-08-09 22:07_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.7±0.42ms     3.8 MB/sec    1.03     11.0±0.31ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.1±0.06ms     7.9 MB/sec    1.00      2.1±0.10ms     8.0 MB/sec
formatter/numpy/globals.py                 1.00    241.1±9.41µs    12.2 MB/sec    1.01   242.5±15.00µs    12.2 MB/sec
formatter/pydantic/types.py                1.01      4.5±0.17ms     5.6 MB/sec    1.00      4.5±0.20ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     13.9±0.33ms     2.9 MB/sec    1.05     14.7±0.33ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.10ms     4.6 MB/sec    1.02      3.7±0.12ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.03   525.5±15.16µs     5.6 MB/sec    1.00   511.6±17.97µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.20ms     3.5 MB/sec    1.04      7.6±0.33ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.03      7.4±0.56ms     5.5 MB/sec    1.00      7.2±0.18ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1510.9±40.12µs    11.0 MB/sec    1.01  1531.6±40.36µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    185.3±7.10µs    15.9 MB/sec    1.00    183.2±7.84µs    16.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.15ms     8.0 MB/sec    1.01      3.2±0.07ms     7.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.8±0.39ms     3.8 MB/sec    1.02     11.0±0.53ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.10ms     8.0 MB/sec    1.02      2.1±0.10ms     7.9 MB/sec
formatter/numpy/globals.py                 1.00   241.4±15.73µs    12.2 MB/sec    1.04   250.2±19.53µs    11.8 MB/sec
formatter/pydantic/types.py                1.01      4.6±0.20ms     5.6 MB/sec    1.00      4.5±0.17ms     5.6 MB/sec
linter/all-rules/large/dataset.py          1.01     14.1±0.39ms     2.9 MB/sec    1.00     14.0±0.42ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.06      4.1±0.24ms     4.1 MB/sec    1.00      3.9±0.13ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.06   527.8±32.98µs     5.6 MB/sec    1.00   498.3±27.55µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.04      7.8±0.24ms     3.3 MB/sec    1.00      7.5±0.38ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      7.6±0.23ms     5.3 MB/sec    1.02      7.8±0.39ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1609.9±81.37µs    10.3 MB/sec    1.00  1593.6±59.76µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   197.4±11.68µs    15.0 MB/sec    1.00   197.2±10.59µs    15.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.19ms     7.4 MB/sec    1.00      3.5±0.23ms     7.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `documentation` added by @zanieb on 2023-08-10 16:28_

---

_@jamesbraza reviewed on 2023-08-10 16:57_

Nice wording, thanks!

---

_@charliermarsh approved on 2023-08-10 22:53_

---

_Merged by @zanieb on 2023-08-11 21:41_

---

_Closed by @zanieb on 2023-08-11 21:41_

---

_Branch deleted on 2023-08-11 21:41_

---
