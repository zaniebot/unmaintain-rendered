```yaml
number: 3955
title: "Check for parenthesis in implicit str concat in `PT006`"
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: main
head: fix/pt006-implicit-str-concat
created_at: 2023-04-13T07:20:37Z
updated_at: 2023-04-14T01:16:17Z
url: https://github.com/astral-sh/ruff/pull/3955
synced_at: 2026-01-12T04:28:19Z
```

# Check for parenthesis in implicit str concat in `PT006`

---

_Pull request opened by @dhruvmanila on 2023-04-13 07:20_

## Summary

The parenthesis aren't part of the AST, so we'll use the lexer. Using the tokens, we'll check if there's a pair of parenthesis present before the first argument. The first argument ends at the first comma, so we can short-circuit.

**Note**: The logic is simple and works only if the name node is a constant string value which is true due to the `match` statement.

The range needs to be computed, even if the fix is not required, to show the correct location in diagnostic. Here, the parenthesis are part of the diagnostic location:

```python
PT006.py:54:26: PT006 [*] Wrong name(s) type in `@pytest.mark.parametrize`, expected `tuple`
   |
54 | @pytest.mark.parametrize(("param1, " "param2, " "param3"), [(1, 2, 3), (4, 5, 6)])
   |                          ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ PT006
55 | def test_implicit_str_concat_with_parens(param1, param2, param3):
56 |     ...
   |
   = help: Use a `tuple` for parameter names
```

fixes: #2918 

---

_Comment by @github-actions[bot] on 2023-04-13 07:34_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.2±0.07ms     2.9 MB/sec    1.00     14.2±0.06ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.6±0.03ms     4.6 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.02    466.3±3.82µs     6.3 MB/sec    1.00    456.9±0.98µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.1±0.04ms     4.2 MB/sec    1.00      6.0±0.03ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.2±0.02ms     5.6 MB/sec    1.00      7.2±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1643.9±3.36µs    10.1 MB/sec    1.00   1617.4±9.19µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.02    182.0±0.44µs    16.2 MB/sec    1.00    179.1±0.44µs    16.5 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.4±0.01ms     7.6 MB/sec    1.00      3.3±0.00ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.1±0.12ms     2.5 MB/sec    1.00     16.1±0.10ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.02ms     4.0 MB/sec    1.00      4.2±0.03ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    440.3±7.96µs     6.7 MB/sec    1.00    434.3±5.24µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.03ms     3.7 MB/sec    1.00      6.9±0.03ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.10ms     4.9 MB/sec    1.03      8.4±0.37ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1787.0±22.01µs     9.3 MB/sec    1.08  1935.5±174.27µs     8.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    185.2±2.20µs    15.9 MB/sec    1.00    185.9±3.71µs    15.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.01ms     6.8 MB/sec    1.04      3.9±0.42ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-04-13 17:51_

Nice idea :)

---

_Merged by @charliermarsh on 2023-04-13 17:56_

---

_Closed by @charliermarsh on 2023-04-13 17:56_

---

_Comment by @dhruvmanila on 2023-04-13 18:32_

> Nice idea :)

Thanks! 

---

_Branch deleted on 2023-04-14 01:16_

---
