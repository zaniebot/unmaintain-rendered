```yaml
number: 4914
title: "ignore if using infinite iterators in `B905`"
type: pull_request
state: merged
author: kyoto7250
labels: []
assignees: []
merged: true
base: main
head: check_itertools_in_zip_without_explicit_strict
created_at: 2023-06-07T02:32:42Z
updated_at: 2023-06-08T02:37:48Z
url: https://github.com/astral-sh/ruff/pull/4914
synced_at: 2026-01-12T15:55:17Z
```

# ignore if using infinite iterators in `B905`

---

_@kyoto7250_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

```python
# https://github.com/PyCQA/flake8-bugbear/issues/378
from itertools import count


for _, _ in zip("abc", count()):
    pass
```

`ruff` raises B905 warning.

```bash
$ ruff --select B905 x.py
x.py:5:13: B905 `zip()` without an explicit `strict=` parameter
Found 1 error.
```

but, if we add `strict=True`, this is invalid code.
```bash
ValueError: zip() argument 2 is longer than argument 1
```



<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

We don't need to warn if `zip` has itertools.count, cycle, repeat. 

I also added test cases.

## Reference
- https://docs.python.org/3/library/itertools.html
- https://github.com/PyCQA/flake8-bugbear/issues/378
- https://github.com/PyCQA/flake8-bugbear/pull/388


This PR was created to have the same behavior as `flake8_bugbear`, but if an infinite iterator is used and `strict=True` is given, it may be better to issue a separate dedicated warning.

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-06-07 02:43_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      5.6±0.02ms     7.3 MB/sec    1.00      5.5±0.01ms     7.4 MB/sec
formatter/numpy/ctypeslib.py               1.02   1097.0±2.76µs    15.2 MB/sec    1.00   1079.8±1.38µs    15.4 MB/sec
formatter/numpy/globals.py                 1.03    125.6±0.14µs    23.5 MB/sec    1.00    121.5±0.29µs    24.3 MB/sec
formatter/pydantic/types.py                1.01      2.4±0.01ms    10.5 MB/sec    1.00      2.4±0.00ms    10.6 MB/sec
linter/all-rules/large/dataset.py          1.00     14.1±0.04ms     2.9 MB/sec    1.01     14.3±0.08ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    423.6±1.55µs     7.0 MB/sec    1.00    423.7±0.87µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.01ms     4.3 MB/sec    1.00      5.9±0.02ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.01ms     6.0 MB/sec    1.01      6.8±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1472.1±2.75µs    11.3 MB/sec    1.00   1465.4±2.28µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.01    164.7±0.24µs    17.9 MB/sec    1.00    162.8±0.80µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.00      3.1±0.00ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.10      7.3±0.06ms     5.5 MB/sec    1.00      6.7±0.05ms     6.1 MB/sec
formatter/numpy/ctypeslib.py               1.09  1404.0±19.27µs    11.9 MB/sec    1.00  1292.1±17.33µs    12.9 MB/sec
formatter/numpy/globals.py                 1.06    151.4±3.06µs    19.5 MB/sec    1.00    142.8±3.06µs    20.7 MB/sec
formatter/pydantic/types.py                1.07      3.2±0.04ms     8.1 MB/sec    1.00      2.9±0.05ms     8.7 MB/sec
linter/all-rules/large/dataset.py          1.00     17.0±0.20ms     2.4 MB/sec    1.00     16.9±0.17ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.08ms     3.9 MB/sec    1.00      4.3±0.04ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   500.5±12.03µs     5.9 MB/sec    1.00    500.8±7.43µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.08ms     3.6 MB/sec    1.01      7.2±0.08ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.07ms     4.9 MB/sec    1.00      8.3±0.06ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1757.2±17.45µs     9.5 MB/sec    1.00  1752.7±19.99µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    201.5±4.22µs    14.6 MB/sec    1.00    202.0±7.75µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.03ms     6.8 MB/sec    1.00      3.7±0.04ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-06-07 03:07_

Thanks! It looks like the bugbear PR also checks whether the `repeat` call has `times=None` (or is omitted): https://github.com/PyCQA/flake8-bugbear/pull/388/files#diff-21e991c5e10f0aca96647b43b4d0968e75c52cc2bceaacf0794f50ded872e49bR1184. Do you think that's worth adding? I don't feel strongly.

---

_Comment by @kyoto7250 on 2023-06-07 03:14_

I missed this conditional branch.

The purpose is to match the behavior with bugbear, so I will add it 👍

---

_Comment by @charliermarsh on 2023-06-08 01:59_

Gonna adjust this real quick so I can include it in the upcoming release.

---

_Merged by @charliermarsh on 2023-06-08 02:12_

---

_Closed by @charliermarsh on 2023-06-08 02:12_

---

_Comment by @kyoto7250 on 2023-06-08 02:33_

I Understood, thanks for your work!

---
