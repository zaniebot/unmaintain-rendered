```yaml
number: 6318
title: "Fix inconsistent `expr_lambda` formatting"
type: pull_request
state: merged
author: qdegraaf
labels:
  - formatter
assignees: []
merged: true
base: main
head: chore/addformatlambdatestcases
created_at: 2023-08-03T20:55:20Z
updated_at: 2023-09-08T21:56:28Z
url: https://github.com/astral-sh/ruff/pull/6318
synced_at: 2026-01-12T02:45:38Z
```

# Fix inconsistent `expr_lambda` formatting

---

_Pull request opened by @qdegraaf on 2023-08-03 20:55_

## Summary
Lambda expression formatting ran into an issue with comments placed like:

```python
(
    lambda
    x:
    # comment 
    x
)
```
Which would be formatted first as:
```python
(
    lambda x: # comment 
    x
)
```
And on a second pass into:
```python
(
    lambda x: x  # comment 
)
```
This was caused by `# comment` changing from a leading comment of the body of the lambda expr to a trailing one after the first pass.

This PR adds test cases for this and similar scenarios and adds additional logic to insert a hard line break if the body has leading comments to ensure consistent formatting.

## Test Plan

Additional test cases added. Comment formatting already deviated from `black` here so it is to be decided if this is the preferred way to handle these comments.

## Issue link

Closes: https://github.com/astral-sh/ruff/issues/6284


---

_Comment by @github-actions[bot] on 2023-08-03 21:30_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.5±0.02ms     4.8 MB/sec    1.00      8.5±0.09ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.01  1705.4±42.39µs     9.8 MB/sec    1.00  1680.3±41.04µs     9.9 MB/sec
formatter/numpy/globals.py                 1.01    174.1±0.77µs    16.9 MB/sec    1.00    172.4±0.62µs    17.1 MB/sec
formatter/pydantic/types.py                1.04      3.8±0.11ms     6.8 MB/sec    1.00      3.6±0.04ms     7.1 MB/sec
linter/all-rules/large/dataset.py          1.02     11.5±0.03ms     3.5 MB/sec    1.00     11.3±0.03ms     3.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.0±0.01ms     5.6 MB/sec    1.00      2.9±0.01ms     5.7 MB/sec
linter/all-rules/numpy/globals.py          1.02    323.8±1.28µs     9.1 MB/sec    1.00    316.8±1.67µs     9.3 MB/sec
linter/all-rules/pydantic/types.py         1.03      5.2±0.01ms     4.9 MB/sec    1.00      5.1±0.02ms     5.0 MB/sec
linter/default-rules/large/dataset.py      1.01      6.0±0.02ms     6.8 MB/sec    1.00      5.9±0.03ms     6.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1210.8±9.42µs    13.8 MB/sec    1.00   1186.6±2.65µs    14.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    121.4±0.97µs    24.3 MB/sec    1.00    121.5±4.94µs    24.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.6±0.01ms     9.6 MB/sec    1.00      2.6±0.01ms     9.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.1±0.47ms     3.4 MB/sec    1.00     12.1±0.40ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.4±0.14ms     7.0 MB/sec    1.00      2.4±0.10ms     7.1 MB/sec
formatter/numpy/globals.py                 1.02   265.0±18.41µs    11.1 MB/sec    1.00   259.2±10.55µs    11.4 MB/sec
formatter/pydantic/types.py                1.04      5.3±0.30ms     4.8 MB/sec    1.00      5.1±0.19ms     5.0 MB/sec
linter/all-rules/large/dataset.py          1.01     16.9±0.56ms     2.4 MB/sec    1.00     16.6±0.47ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.21ms     3.8 MB/sec    1.00      4.5±0.26ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.05   559.7±39.54µs     5.3 MB/sec    1.00   533.0±26.22µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.7±0.28ms     3.3 MB/sec    1.00      7.5±0.28ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      9.0±0.31ms     4.5 MB/sec    1.01      9.1±0.32ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1849.1±70.04µs     9.0 MB/sec    1.00  1824.2±58.86µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.02   217.0±22.41µs    13.6 MB/sec    1.00   212.2±14.56µs    13.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.10ms     6.5 MB/sec    1.01      4.0±0.15ms     6.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `formatter` added by @konstin on 2023-08-04 14:16_

---

_Renamed from "Expand test cases for expr_lambda formatting" to "Fix inconsistent `expr_lambda` formatting" by @qdegraaf on 2023-09-07 14:35_

---

_Marked ready for review by @qdegraaf on 2023-09-07 14:41_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_lambda.rs`:48 on 2023-09-08 06:34_

```suggestion
        if comments.has_leading(body) {
```

---

_@MichaReiser approved on 2023-09-08 06:34_

Thanks for adding more test coverage and fixing the stability bug. I only have a small nit

---

_@konstin approved on 2023-09-08 08:48_

---

_Merged by @MichaReiser on 2023-09-08 09:40_

---

_Closed by @MichaReiser on 2023-09-08 09:40_

---

_Branch deleted on 2023-09-08 21:56_

---
