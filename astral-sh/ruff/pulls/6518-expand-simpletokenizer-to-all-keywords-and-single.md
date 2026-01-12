```yaml
number: 6518
title: "Expand `SimpleTokenizer` to all keywords and single-character tokens"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/tokenizer
created_at: 2023-08-12T03:39:41Z
updated_at: 2023-08-14T14:35:32Z
url: https://github.com/astral-sh/ruff/pull/6518
synced_at: 2026-01-12T02:52:04Z
```

# Expand `SimpleTokenizer` to all keywords and single-character tokens

---

_Pull request opened by @charliermarsh on 2023-08-12 03:39_

## Summary

For #6485, I need to be able to use the `SimpleTokenizer` to lex the space between any two adjacent expressions (i.e., the space between a preceding and following node). This requires that we support a wider range of keywords (like `and`, to connect the pieces of `x and y`), and some additional single-character tokens (like `-` and `>`, to support `->`). Note that the `SimpleTokenizer` does not support multi-character tokens, so the `->` in a function signature is lexed as a `-` followed by a `>` -- but this is fine for our purposes.


---

_Label `internal` added by @charliermarsh on 2023-08-12 03:39_

---

_Comment by @github-actions[bot] on 2023-08-12 03:58_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.4±0.02ms     4.9 MB/sec    1.00      8.3±0.02ms     4.9 MB/sec
formatter/numpy/ctypeslib.py               1.00   1631.6±3.18µs    10.2 MB/sec    1.00   1630.1±5.41µs    10.2 MB/sec
formatter/numpy/globals.py                 1.01    172.6±0.24µs    17.1 MB/sec    1.00    171.7±0.82µs    17.2 MB/sec
formatter/pydantic/types.py                1.01      3.6±0.03ms     7.2 MB/sec    1.00      3.5±0.01ms     7.3 MB/sec
linter/all-rules/large/dataset.py          1.02     10.8±0.01ms     3.8 MB/sec    1.00     10.5±0.02ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      2.9±0.00ms     5.7 MB/sec    1.00      2.9±0.01ms     5.7 MB/sec
linter/all-rules/numpy/globals.py          1.02    333.1±1.31µs     8.9 MB/sec    1.00    326.8±1.84µs     9.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.5±0.01ms     4.6 MB/sec    1.00      5.5±0.04ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.04      5.8±0.01ms     7.1 MB/sec    1.00      5.5±0.01ms     7.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03   1223.9±1.29µs    13.6 MB/sec    1.00   1187.2±4.10µs    14.0 MB/sec
linter/default-rules/numpy/globals.py      1.02    127.3±0.81µs    23.2 MB/sec    1.00    124.7±0.42µs    23.7 MB/sec
linter/default-rules/pydantic/types.py     1.03      2.6±0.01ms     9.9 MB/sec    1.00      2.5±0.00ms    10.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.08ms     4.1 MB/sec    1.01      9.9±0.10ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1891.7±21.73µs     8.8 MB/sec    1.01  1915.2±33.28µs     8.7 MB/sec
formatter/numpy/globals.py                 1.00    207.9±3.40µs    14.2 MB/sec    1.02    212.4±7.07µs    13.9 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.05ms     6.2 MB/sec    1.01      4.1±0.06ms     6.2 MB/sec
linter/all-rules/large/dataset.py          1.00     12.4±0.08ms     3.3 MB/sec    1.01     12.5±0.12ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.03ms     4.9 MB/sec    1.00      3.4±0.03ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    427.5±8.12µs     6.9 MB/sec    1.00    427.5±7.44µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.04ms     4.0 MB/sec    1.01      6.4±0.11ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.06ms     6.0 MB/sec    1.01      6.8±0.05ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1439.8±12.95µs    11.6 MB/sec    1.00  1442.1±16.91µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    168.4±2.52µs    17.5 MB/sec    1.01    169.6±3.12µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.04ms     8.5 MB/sec    1.00      3.0±0.03ms     8.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-08-14 07:14_

The SimpleTokenizer can lex multi-character tokens, it just requires a manual implementation. 

I recommend implementing `->` as a single token instead of two.

---

_Comment by @charliermarsh on 2023-08-14 14:35_

> I recommend implementing `->` as a single token instead of two.

I will do this in a separate PR, as there are a bunch of these (like `+=`).

---

_Merged by @charliermarsh on 2023-08-14 14:35_

---

_Closed by @charliermarsh on 2023-08-14 14:35_

---

_Branch deleted on 2023-08-14 14:35_

---
