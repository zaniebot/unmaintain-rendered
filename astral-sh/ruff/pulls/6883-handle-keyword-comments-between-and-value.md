```yaml
number: 6883
title: Handle keyword comments between = and value
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/keyword-comments
created_at: 2023-08-25T21:10:27Z
updated_at: 2023-08-30T13:53:18Z
url: https://github.com/astral-sh/ruff/pull/6883
synced_at: 2026-01-12T02:45:38Z
```

# Handle keyword comments between = and value

---

_Pull request opened by @charliermarsh on 2023-08-25 21:10_

## Summary

This PR adds comment handling for comments between the `=` and the `value` for keywords, as in the following cases:

```python
func(
    x  # dangling
    =  # dangling
    # dangling
    1,
    **  # dangling
    y
)
```

(Comments after the `**` were already handled in some cases, but I've unified the handling with the `=` handling.)

Note that, previously, comments between the `**` and its value were rendered as trailing comments on the value (so they'd appear after `y`). This struck me as odd since it effectively re-ordered the comment with respect to its closest AST node (the value). I've made them leading comments, though I don't know that that's a significant improvement. I could also imagine us leaving them where they are.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-25 21:10_

---

_Label `formatter` added by @charliermarsh on 2023-08-25 21:10_

---

_Comment by @github-actions[bot] on 2023-08-25 21:48_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      4.9±0.51ms     8.4 MB/sec     1.01      4.9±0.22ms     8.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   934.6±45.33µs    17.8 MB/sec     1.06   990.1±70.65µs    16.8 MB/sec
formatter/numpy/globals.py                 1.00     85.3±4.32µs    34.6 MB/sec     1.06     90.1±9.72µs    32.7 MB/sec
formatter/pydantic/types.py                1.00  1785.1±90.59µs    14.3 MB/sec     1.04  1849.0±131.54µs    13.8 MB/sec
linter/all-rules/large/dataset.py          1.01     12.3±0.59ms     3.3 MB/sec     1.00     12.2±0.93ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.3±0.20ms     5.0 MB/sec     1.00      3.3±0.25ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   450.7±24.90µs     6.5 MB/sec     1.05   475.1±45.26µs     6.2 MB/sec
linter/all-rules/pydantic/types.py         1.03      6.5±0.30ms     4.0 MB/sec     1.00      6.2±0.41ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.09      6.8±0.46ms     6.0 MB/sec     1.00      6.2±0.40ms     6.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04  1463.9±117.76µs    11.4 MB/sec    1.00  1406.8±117.38µs    11.8 MB/sec
linter/default-rules/numpy/globals.py      1.00   169.5±10.93µs    17.4 MB/sec     1.02   172.7±10.57µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.06      2.9±0.19ms     8.6 MB/sec     1.00      2.8±0.14ms     9.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.2±0.18ms     6.5 MB/sec    1.00      6.2±0.23ms     6.5 MB/sec
formatter/numpy/ctypeslib.py               1.00  1217.5±47.29µs    13.7 MB/sec    1.00  1219.4±60.36µs    13.7 MB/sec
formatter/numpy/globals.py                 1.00    115.2±7.81µs    25.6 MB/sec    1.00    115.3±8.29µs    25.6 MB/sec
formatter/pydantic/types.py                1.00      2.4±0.11ms    10.8 MB/sec    1.00      2.4±0.08ms    10.8 MB/sec
linter/all-rules/large/dataset.py          1.00     15.3±0.36ms     2.7 MB/sec    1.00     15.3±0.51ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.12ms     4.0 MB/sec    1.01      4.2±0.18ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   529.5±32.63µs     5.6 MB/sec    1.00   530.4±38.07µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.0±0.23ms     3.2 MB/sec    1.00      7.9±0.24ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.16ms     4.8 MB/sec    1.00      8.4±0.24ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1803.2±62.46µs     9.2 MB/sec    1.00  1805.1±66.42µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.01   214.2±11.24µs    13.8 MB/sec    1.00   211.5±20.30µs    14.0 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.8±0.10ms     6.7 MB/sec    1.00      3.8±0.11ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @codspeed-hq[bot] on 2023-08-26 16:05_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/keyword-comments)

### Merging #6883 will **not alter performance**

<sub>Comparing <code>charlie/keyword-comments</code> (b88198c) with <code>main</code> (15b73bd)</sub>



### Summary

`✅ 16` untouched benchmarks






---

_Comment by @charliermarsh on 2023-08-30 13:40_

Friendly ping @MichaReiser, sorry about this one.

---

_@MichaReiser approved on 2023-08-30 13:52_

---

_Comment by @MichaReiser on 2023-08-30 13:52_

Can you do a quick test run to ensure the similarity indices are only improving? 

---

_Merged by @charliermarsh on 2023-08-30 13:52_

---

_Closed by @charliermarsh on 2023-08-30 13:52_

---

_Branch deleted on 2023-08-30 13:52_

---

_Comment by @charliermarsh on 2023-08-30 13:53_

Ugh sorry I missed your comment.

---

_Comment by @charliermarsh on 2023-08-30 13:53_

I will test it manually.

---
