```yaml
number: 5469
title: "[`flake8-django`] Skip duplicate violations in `DJ012`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/DJ012
created_at: 2023-07-03T00:56:36Z
updated_at: 2023-07-03T01:26:42Z
url: https://github.com/astral-sh/ruff/pull/5469
synced_at: 2026-01-12T15:55:18Z
```

# [`flake8-django`] Skip duplicate violations in `DJ012`

---

_@charliermarsh_

## Summary

This PR reduces the noise from `DJ012` by emitting a single violation when you have multiple consecutive violations of the same "type".

For example, given:

```py
class MultipleConsecutiveFields(models.Model):
    """Model that contains multiple out-of-order field definitions in a row."""


    class Meta:
        verbose_name = "test"

    first_name = models.CharField(max_length=32)
    last_name = models.CharField(max_length=32)
```

It's convenient to only error on `first_name`, and not `last_name`, since we're really flagging that the _section_ is out-of-order.

Closes #5465.


---

_Renamed from "Skip duplicate violations in DJ012" to "[`flake8-django`] Skip duplicate violations in `DJ012`" by @charliermarsh on 2023-07-03 00:56_

---

_Label `rule` added by @charliermarsh on 2023-07-03 00:56_

---

_Comment by @github-actions[bot] on 2023-07-03 01:05_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.08     10.0±0.51ms     4.1 MB/sec    1.00      9.3±0.27ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.13      2.3±0.13ms     7.4 MB/sec    1.00  1988.2±69.25µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00   228.6±12.78µs    12.9 MB/sec    1.04   238.9±13.93µs    12.4 MB/sec
formatter/pydantic/types.py                1.03      4.5±0.16ms     5.7 MB/sec    1.00      4.3±0.17ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.13     18.6±0.91ms     2.2 MB/sec    1.00     16.5±0.40ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.07      4.3±0.27ms     3.9 MB/sec    1.00      4.0±0.14ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.01   537.2±27.12µs     5.5 MB/sec    1.00   534.3±27.16µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.08      7.7±0.26ms     3.3 MB/sec    1.00      7.1±0.18ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.14      9.4±0.28ms     4.3 MB/sec    1.00      8.3±0.23ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.14      2.0±0.08ms     8.3 MB/sec    1.00  1760.9±57.11µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.09    225.0±9.27µs    13.1 MB/sec    1.00   206.0±12.53µs    14.3 MB/sec
linter/default-rules/pydantic/types.py     1.14      4.3±0.21ms     5.9 MB/sec    1.00      3.8±0.16ms     6.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     11.7±0.37ms     3.5 MB/sec    1.00     11.6±0.39ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.5±0.11ms     6.6 MB/sec    1.00      2.5±0.12ms     6.7 MB/sec
formatter/numpy/globals.py                 1.03   297.9±24.90µs     9.9 MB/sec    1.00   289.7±20.67µs    10.2 MB/sec
formatter/pydantic/types.py                1.02      5.5±0.23ms     4.6 MB/sec    1.00      5.4±0.24ms     4.7 MB/sec
linter/all-rules/large/dataset.py          1.00     19.4±0.52ms     2.1 MB/sec    1.00     19.4±0.52ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      5.2±0.20ms     3.2 MB/sec    1.00      5.0±0.16ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   632.1±32.92µs     4.7 MB/sec    1.00   629.5±28.84µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.03      8.8±0.32ms     2.9 MB/sec    1.00      8.5±0.27ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00     10.0±0.31ms     4.1 MB/sec    1.00     10.0±0.42ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03      2.2±0.10ms     7.6 MB/sec    1.00      2.1±0.08ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.00   251.8±12.41µs    11.7 MB/sec    1.00   252.5±12.96µs    11.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.6±0.20ms     5.6 MB/sec    1.00      4.5±0.18ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-03 01:09_

---

_Closed by @charliermarsh on 2023-07-03 01:09_

---

_Branch deleted on 2023-07-03 01:09_

---
