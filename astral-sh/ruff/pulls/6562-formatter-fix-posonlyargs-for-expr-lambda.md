```yaml
number: 6562
title: "Formatter: Fix posonlyargs for `expr_lambda`"
type: pull_request
state: merged
author: qdegraaf
labels:
  - formatter
assignees: []
merged: true
base: main
head: fix/lambdaformat
created_at: 2023-08-14T15:01:19Z
updated_at: 2023-08-14T15:42:10Z
url: https://github.com/astral-sh/ruff/pull/6562
synced_at: 2026-01-12T15:55:21Z
```

# Formatter: Fix posonlyargs for `expr_lambda`

---

_@qdegraaf_

## Summary

Looked like the conditional check for writing the body was not complete enough to cover cases where `/` is the last param of a lambda like in:
```python
lambda x, /: x
```

Thus the formatter was not writing the parameters. This PR expands the check to also write the params if  `!parameters.posonlyargs.is_empty()`

## Test Plan

Additional cases with `/` arg were added to `lambda.py` fixture.

If I understand correctly the black_compatibility snapshot change is good as that means we now handle these cases the same as black. Let me know if I misunderstand that and something is off about this change.

## Issue link
Closes: https://github.com/astral-sh/ruff/issues/6560


---

_@MichaReiser approved on 2023-08-14 15:38_

Nice! Now @konstin can enjoy their time off. It was bugging him, why this only worked for functions ;)

---

_Label `formatter` added by @MichaReiser on 2023-08-14 15:38_

---

_Merged by @MichaReiser on 2023-08-14 15:38_

---

_Closed by @MichaReiser on 2023-08-14 15:38_

---

_Comment by @github-actions[bot] on 2023-08-14 15:42_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      4.3±0.19ms     9.5 MB/sec    1.00      4.2±0.17ms     9.7 MB/sec
formatter/numpy/ctypeslib.py               1.04   851.8±37.04µs    19.5 MB/sec    1.00   820.1±33.23µs    20.3 MB/sec
formatter/numpy/globals.py                 1.00     81.5±3.19µs    36.2 MB/sec    1.03     84.0±3.46µs    35.1 MB/sec
formatter/pydantic/types.py                1.00  1658.7±46.72µs    15.4 MB/sec    1.03  1713.2±76.44µs    14.9 MB/sec
linter/all-rules/large/dataset.py          1.00     11.3±0.27ms     3.6 MB/sec    1.01     11.5±0.24ms     3.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.1±0.08ms     5.4 MB/sec    1.00      3.1±0.07ms     5.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   437.7±13.58µs     6.7 MB/sec    1.00   436.8±14.38µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.14ms     4.3 MB/sec    1.01      6.0±0.14ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.0±0.11ms     6.8 MB/sec    1.00      6.0±0.14ms     6.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1331.1±38.33µs    12.5 MB/sec    1.03  1376.8±46.42µs    12.1 MB/sec
linter/default-rules/numpy/globals.py      1.06   164.9±10.00µs    17.9 MB/sec    1.00    155.9±4.85µs    18.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.7±0.08ms     9.5 MB/sec    1.02      2.8±0.06ms     9.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.6±0.36ms     7.3 MB/sec    1.08      6.0±0.41ms     6.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1022.9±62.63µs    16.3 MB/sec    1.09  1111.2±94.15µs    15.0 MB/sec
formatter/numpy/globals.py                 1.00    101.5±8.63µs    29.1 MB/sec    1.10   111.5±13.27µs    26.5 MB/sec
formatter/pydantic/types.py                1.00      2.1±0.17ms    12.2 MB/sec    1.11      2.3±0.21ms    11.0 MB/sec
linter/all-rules/large/dataset.py          1.00     17.6±0.65ms     2.3 MB/sec    1.04     18.4±0.66ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.0±0.30ms     3.3 MB/sec    1.02      5.1±0.35ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   654.6±49.43µs     4.5 MB/sec    1.06  691.1±149.40µs     4.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.8±0.52ms     2.6 MB/sec    1.02     10.0±0.58ms     2.5 MB/sec
linter/default-rules/large/dataset.py      1.00     10.7±0.58ms     3.8 MB/sec    1.01     10.7±0.55ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.2±0.21ms     7.5 MB/sec    1.00      2.2±0.11ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   256.9±18.10µs    11.5 MB/sec    1.03   264.6±15.00µs    11.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.21ms     5.7 MB/sec    1.04      4.6±0.22ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
