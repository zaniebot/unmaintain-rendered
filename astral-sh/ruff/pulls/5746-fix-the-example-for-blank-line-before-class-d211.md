```yaml
number: 5746
title: Fix the example for blank-line-before-class (D211)
type: pull_request
state: merged
author: skykasko
labels:
  - documentation
assignees: []
merged: true
base: main
head: sky/fix-d11-example
created_at: 2023-07-13T17:34:23Z
updated_at: 2023-07-13T18:11:29Z
url: https://github.com/astral-sh/ruff/pull/5746
synced_at: 2026-01-12T15:55:19Z
```

# Fix the example for blank-line-before-class (D211)

---

_@skykasko_

The example for [D211](https://beta.ruff.rs/docs/rules/blank-line-before-class/) is currently identical to the example for [D203](https://beta.ruff.rs/docs/rules/one-blank-line-before-class/). It should be the opposite, with the incorrect case having a blank line before the class docstring and the correct case having no blank line.



---

_Comment by @charliermarsh on 2023-07-13 17:36_

Thanks!

---

_Label `documentation` added by @charliermarsh on 2023-07-13 17:36_

---

_Merged by @charliermarsh on 2023-07-13 17:47_

---

_Closed by @charliermarsh on 2023-07-13 17:47_

---

_Branch deleted on 2023-07-13 17:52_

---

_Comment by @github-actions[bot] on 2023-07-13 17:57_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     11.0±0.40ms     3.7 MB/sec    1.00     10.9±0.38ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.5±0.09ms     6.7 MB/sec    1.00      2.5±0.27ms     6.7 MB/sec
formatter/numpy/globals.py                 1.01   287.4±22.01µs    10.3 MB/sec    1.00   284.4±18.01µs    10.4 MB/sec
formatter/pydantic/types.py                1.03      5.5±0.23ms     4.6 MB/sec    1.00      5.3±0.23ms     4.8 MB/sec
linter/all-rules/large/dataset.py          1.00     19.6±0.46ms     2.1 MB/sec    1.01     19.7±0.42ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.8±0.17ms     3.5 MB/sec    1.00      4.8±0.18ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.00   619.9±27.74µs     4.8 MB/sec    1.04   641.7±32.09µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.7±0.28ms     2.9 MB/sec    1.00      8.8±0.31ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00      9.4±0.23ms     4.3 MB/sec    1.03      9.6±0.24ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.09ms     8.3 MB/sec    1.02      2.0±0.07ms     8.1 MB/sec
linter/default-rules/numpy/globals.py      1.00   237.8±16.98µs    12.4 MB/sec    1.04    247.8±9.74µs    11.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.2±0.19ms     6.1 MB/sec    1.03      4.3±0.15ms     5.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.7±0.42ms     3.5 MB/sec    1.00     11.7±0.40ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.03      2.7±0.13ms     6.2 MB/sec    1.00      2.6±0.12ms     6.3 MB/sec
formatter/numpy/globals.py                 1.02   309.1±20.44µs     9.5 MB/sec    1.00   302.4±16.49µs     9.8 MB/sec
formatter/pydantic/types.py                1.01      5.8±0.27ms     4.4 MB/sec    1.00      5.7±0.24ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.00     19.8±0.51ms     2.1 MB/sec    1.01     20.0±0.57ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.21ms     3.2 MB/sec    1.03      5.3±0.25ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   620.8±30.13µs     4.8 MB/sec    1.00   621.1±32.81µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.9±0.32ms     2.9 MB/sec    1.02      9.1±0.43ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.0±0.34ms     4.1 MB/sec    1.00     10.0±0.33ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.09ms     7.8 MB/sec    1.01      2.2±0.10ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   246.2±10.99µs    12.0 MB/sec    1.03   254.2±17.07µs    11.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.14ms     5.7 MB/sec    1.01      4.5±0.16ms     5.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
