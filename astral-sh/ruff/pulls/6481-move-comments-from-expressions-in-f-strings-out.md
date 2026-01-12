```yaml
number: 6481
title: move comments from expressions in f-strings out
type: pull_request
state: merged
author: davidszotten
labels:
  - formatter
assignees: []
merged: true
base: main
head: fix-more-dropped-fstring-comments
created_at: 2023-08-10T16:07:08Z
updated_at: 2023-08-11T07:22:37Z
url: https://github.com/astral-sh/ruff/pull/6481
synced_at: 2026-01-12T02:52:04Z
```

# move comments from expressions in f-strings out

---

_Pull request opened by @davidszotten on 2023-08-10 16:07_

move comments from expressions inside f-strings out to the containing f-string.

`py<3.12` doesn't support comments inside formatted values. once we support 3.12+ f-strings we may need to revisit this. i suspect we'll end up with chunks of different behaviour for pre vs post 3.12 f-strings and this will be part of that.

fixes #6476 

---

_Comment by @github-actions[bot] on 2023-08-10 16:46_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.0±0.10ms     4.1 MB/sec    1.00      9.9±0.07ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.01  1961.8±25.18µs     8.5 MB/sec    1.00   1948.0±5.51µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00   226.4±15.13µs    13.0 MB/sec    1.04   236.0±23.37µs    12.5 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.06ms     6.1 MB/sec    1.01      4.2±0.03ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     12.6±0.18ms     3.2 MB/sec    1.01     12.7±0.10ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.07ms     5.0 MB/sec    1.03      3.4±0.16ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    459.6±1.57µs     6.4 MB/sec    1.00    459.4±0.98µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.12ms     3.9 MB/sec    1.02      6.7±0.06ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.01      6.5±0.05ms     6.2 MB/sec    1.00      6.5±0.06ms     6.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1388.6±5.17µs    12.0 MB/sec    1.00   1376.0±2.59µs    12.1 MB/sec
linter/default-rules/numpy/globals.py      1.02    160.6±4.38µs    18.4 MB/sec    1.00    157.6±0.29µs    18.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.02ms     8.8 MB/sec    1.00      2.9±0.02ms     8.8 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.02     13.5±0.81ms     3.0 MB/sec     1.00     13.2±0.84ms     3.1 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.5±0.18ms     6.6 MB/sec     1.02      2.6±0.19ms     6.5 MB/sec
formatter/numpy/globals.py                 1.00   291.5±26.56µs    10.1 MB/sec     1.07   311.9±26.59µs     9.5 MB/sec
formatter/pydantic/types.py                1.04      5.9±0.43ms     4.4 MB/sec     1.00      5.6±0.37ms     4.5 MB/sec
linter/all-rules/large/dataset.py          1.00     18.0±0.75ms     2.3 MB/sec     1.02     18.4±0.80ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.09      5.1±0.37ms     3.2 MB/sec     1.00      4.7±0.40ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.04   635.5±36.51µs     4.6 MB/sec     1.00   609.2±34.73µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.08      9.8±0.50ms     2.6 MB/sec     1.00      9.1±0.66ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.05     10.2±0.58ms     4.0 MB/sec     1.00      9.7±0.64ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1921.4±108.11µs     8.7 MB/sec    1.05      2.0±0.13ms     8.3 MB/sec
linter/default-rules/numpy/globals.py      1.06   256.3±25.63µs    11.5 MB/sec     1.00   242.7±22.94µs    12.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.25ms     6.0 MB/sec     1.07      4.5±0.35ms     5.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin approved on 2023-08-10 16:48_

---

_Review requested from @MichaReiser by @konstin on 2023-08-10 16:50_

---

_@MichaReiser approved on 2023-08-11 07:22_

---

_Comment by @MichaReiser on 2023-08-11 07:22_

Thanks

---

_Merged by @MichaReiser on 2023-08-11 07:22_

---

_Closed by @MichaReiser on 2023-08-11 07:22_

---

_Label `formatter` added by @MichaReiser on 2023-08-11 07:22_

---
