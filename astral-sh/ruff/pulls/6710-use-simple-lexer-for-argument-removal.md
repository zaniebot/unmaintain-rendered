```yaml
number: 6710
title: Use simple lexer for argument removal
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/lexer
created_at: 2023-08-21T04:08:16Z
updated_at: 2023-08-21T06:59:35Z
url: https://github.com/astral-sh/ruff/pull/6710
synced_at: 2026-01-12T02:52:04Z
```

# Use simple lexer for argument removal

---

_Pull request opened by @charliermarsh on 2023-08-21 04:08_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2023-08-21 04:08_

---

_Merged by @charliermarsh on 2023-08-21 04:16_

---

_Closed by @charliermarsh on 2023-08-21 04:16_

---

_Branch deleted on 2023-08-21 04:16_

---

_Comment by @github-actions[bot] on 2023-08-21 04:19_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      3.3±0.02ms    12.3 MB/sec    1.00      3.3±0.02ms    12.4 MB/sec
formatter/numpy/ctypeslib.py               1.00    688.1±2.53µs    24.2 MB/sec    1.00    687.6±2.36µs    24.2 MB/sec
formatter/numpy/globals.py                 1.00     72.3±0.26µs    40.8 MB/sec    1.00     72.4±1.10µs    40.8 MB/sec
formatter/pydantic/types.py                1.01  1351.2±23.11µs    18.9 MB/sec    1.00   1336.9±9.68µs    19.1 MB/sec
linter/all-rules/large/dataset.py          1.00     10.1±0.09ms     4.0 MB/sec    1.00     10.1±0.03ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.01ms     6.1 MB/sec    1.00      2.7±0.01ms     6.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    393.3±2.66µs     7.5 MB/sec    1.00    390.5±0.41µs     7.6 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.3±0.06ms     4.8 MB/sec    1.00      5.3±0.04ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.01      5.4±0.04ms     7.5 MB/sec    1.00      5.4±0.01ms     7.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1197.4±8.61µs    13.9 MB/sec    1.00  1191.3±18.25µs    14.0 MB/sec
linter/default-rules/numpy/globals.py      1.02    141.4±1.92µs    20.9 MB/sec    1.00    139.1±1.01µs    21.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.5±0.05ms    10.3 MB/sec    1.00      2.5±0.05ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      4.7±0.30ms     8.7 MB/sec     1.01      4.7±0.26ms     8.6 MB/sec
formatter/numpy/ctypeslib.py               1.02   932.7±43.69µs    17.9 MB/sec     1.00   917.5±42.26µs    18.1 MB/sec
formatter/numpy/globals.py                 1.00     93.9±6.57µs    31.4 MB/sec     1.00     93.7±6.28µs    31.5 MB/sec
formatter/pydantic/types.py                1.00  1882.8±109.14µs    13.5 MB/sec    1.00  1881.5±151.08µs    13.6 MB/sec
linter/all-rules/large/dataset.py          1.00     18.1±0.37ms     2.2 MB/sec     1.00     18.1±0.67ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      5.0±0.33ms     3.4 MB/sec     1.00      4.8±0.21ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.01   596.5±17.57µs     4.9 MB/sec     1.00   587.8±22.05µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.5±0.31ms     2.7 MB/sec     1.00      9.5±0.35ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.00      9.7±0.33ms     4.2 MB/sec     1.02      9.9±0.33ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04      2.1±0.10ms     8.1 MB/sec     1.00  1988.8±52.68µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   250.3±13.68µs    11.8 MB/sec     1.02   254.5±23.71µs    11.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.16ms     6.0 MB/sec     1.00      4.3±0.13ms     5.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @MichaReiser on 2023-08-21 06:59_

What was the motivation for this change? 

I'm asking because we probably want to merge our tokenizers in the longterm (it doesn't make sense to have too). I only introduced the `SimpleTokenizer` because our normal Tokenizer (and tokens) allocate, something I wanted to avoid in the formatter's hot code path. But this isn't really an issue during fixes.

---
