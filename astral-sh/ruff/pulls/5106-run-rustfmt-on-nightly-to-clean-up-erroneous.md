```yaml
number: 5106
title: "Run `rustfmt` on nightly to clean up erroneous comments"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/rustfmt
created_at: 2023-06-15T00:10:36Z
updated_at: 2023-06-15T00:37:33Z
url: https://github.com/astral-sh/ruff/pull/5106
synced_at: 2026-01-12T03:43:30Z
```

# Run `rustfmt` on nightly to clean up erroneous comments

---

_Pull request opened by @charliermarsh on 2023-06-15 00:10_

## Summary

This PR runs `rustfmt` with a few nightly options as a one-time fix to catch some malformatted comments. I ended up just running with:

```toml
condense_wildcard_suffixes = true
edition = "2021"
max_width = 100
normalize_comments = true
normalize_doc_attributes = true
reorder_impl_items = true
unstable_features = true
use_field_init_shorthand = true
```

Since these all seem like reasonable things to fix, so may as well while I'm here.


---

_Label `internal` added by @charliermarsh on 2023-06-15 00:10_

---

_Merged by @charliermarsh on 2023-06-15 00:19_

---

_Closed by @charliermarsh on 2023-06-15 00:19_

---

_Branch deleted on 2023-06-15 00:19_

---

_Comment by @github-actions[bot] on 2023-06-15 00:22_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      7.6±0.06ms     5.3 MB/sec    1.00      7.5±0.02ms     5.4 MB/sec
formatter/numpy/ctypeslib.py               1.01   1598.6±4.12µs    10.4 MB/sec    1.00   1581.8±5.01µs    10.5 MB/sec
formatter/numpy/globals.py                 1.00    152.6±1.03µs    19.3 MB/sec    1.00    152.8±2.99µs    19.3 MB/sec
formatter/pydantic/types.py                1.01      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.01ms     8.3 MB/sec
linter/all-rules/large/dataset.py          1.00     16.3±0.07ms     2.5 MB/sec    1.00     16.3±0.08ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.01ms     4.2 MB/sec    1.00      4.0±0.01ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    501.2±0.68µs     5.9 MB/sec    1.01    504.4±5.35µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.03ms     3.7 MB/sec    1.00      7.0±0.02ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.02ms     5.1 MB/sec    1.01      8.1±0.02ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1748.9±2.63µs     9.5 MB/sec    1.01   1761.1±4.19µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    198.3±0.50µs    14.9 MB/sec    1.01    200.8±4.23µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.00ms     7.0 MB/sec    1.01      3.7±0.00ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.0±0.29ms     5.8 MB/sec    1.05      7.4±0.32ms     5.5 MB/sec
formatter/numpy/ctypeslib.py               1.00  1442.8±70.52µs    11.5 MB/sec    1.06  1531.0±81.27µs    10.9 MB/sec
formatter/numpy/globals.py                 1.00    134.9±7.38µs    21.9 MB/sec    1.08   146.1±13.87µs    20.2 MB/sec
formatter/pydantic/types.py                1.00      2.9±0.14ms     8.7 MB/sec    1.00      2.9±0.13ms     8.7 MB/sec
linter/all-rules/large/dataset.py          1.00     15.2±0.67ms     2.7 MB/sec    1.00     15.2±0.64ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.23ms     4.3 MB/sec    1.03      3.9±0.17ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.02   465.0±39.32µs     6.3 MB/sec    1.00   457.6±25.51µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.6±0.25ms     3.9 MB/sec    1.00      6.5±0.28ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.01      7.9±0.42ms     5.1 MB/sec    1.00      7.9±0.52ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1606.0±72.02µs    10.4 MB/sec    1.02  1645.4±84.50µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00   182.6±21.38µs    16.2 MB/sec    1.00   183.3±10.94µs    16.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.5±0.14ms     7.4 MB/sec    1.00      3.4±0.14ms     7.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
