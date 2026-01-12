```yaml
number: 6389
title: Expand parents whenever open-parenthesis comments are present
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/expand-parent
created_at: 2023-08-07T14:09:56Z
updated_at: 2023-08-08T12:45:22Z
url: https://github.com/astral-sh/ruff/pull/6389
synced_at: 2026-01-12T15:55:21Z
```

# Expand parents whenever open-parenthesis comments are present

---

_@charliermarsh_

## Summary

This PR modifies our dangling-open-parenthesis handling to _always_ expand the parent expression.

So, for example, given:

```python
a = int(  # type: ignore
    int(  # type: ignore
        int(  # type: ignore
            6
        )
    )
)
```

We now retain that as stable formatting, instead of truncating like:

```python
a = int(int(int(6)))  # comment  # comment  # comment
```

Note that Black _does_ collapse comments like this _unless_ they're `# type: ignore` comments, and perhaps in some other cases, so this is an intentional deviation ([playground](https://black.vercel.app/?version=main&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4AFEAHpdAD2IimZxl1N_WlOfrjryFgvD4ScVsKPztqdHDGJUg5knO0JCdpUfW1IrWSNmIJPx95s0hP-pRNkCQNH64-eIznIvXjeWBQ5-qax0oNw4yMOuhwr2azvMRZaEB5r8IXVPHmRCJp7fe7y4290u1zzxqK_nAi6q_5sI-jsAAAAA8HgZ9V7hG3QAAZYBxQIAAGnCHXexxGf7AgAAAAAEWVo=)).

---

_Marked ready for review by @charliermarsh on 2023-08-07 14:16_

---

_Converted to draft by @charliermarsh on 2023-08-07 14:17_

---

_Marked ready for review by @charliermarsh on 2023-08-07 14:20_

---

_Comment by @charliermarsh on 2023-08-07 14:21_

We still get this wrong:

```python
a = (  # type: ignore
    int(  # type: ignore
        int(  # type: ignore
            int(  # type: ignore
                6
            )
        )
    )
)
```

Formatting as:

```python
a = int(  # type: ignore  # type: ignore
    int(  # type: ignore
        int(  # type: ignore
            6
        )
    )
)
```

But I think that's a separate issue.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-07 14:21_

---

_Review requested from @konstin by @charliermarsh on 2023-08-07 14:21_

---

_Comment by @github-actions[bot] on 2023-08-07 14:51_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.25      9.8±0.36ms     4.2 MB/sec     1.00      7.8±0.31ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.22  1902.8±121.34µs     8.8 MB/sec    1.00  1555.4±96.14µs    10.7 MB/sec
formatter/numpy/globals.py                 1.02   185.5±12.43µs    15.9 MB/sec     1.00   182.8±12.87µs    16.1 MB/sec
formatter/pydantic/types.py                1.15      3.9±0.32ms     6.5 MB/sec     1.00      3.4±0.21ms     7.5 MB/sec
linter/all-rules/large/dataset.py          1.27     13.5±0.28ms     3.0 MB/sec     1.00     10.7±0.56ms     3.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.24      3.5±0.11ms     4.7 MB/sec     1.00      2.8±0.14ms     5.9 MB/sec
linter/all-rules/numpy/globals.py          1.16   481.2±29.13µs     6.1 MB/sec     1.00   416.4±41.39µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.21      5.9±0.21ms     4.3 MB/sec     1.00      4.8±0.20ms     5.3 MB/sec
linter/default-rules/large/dataset.py      1.17      6.3±0.20ms     6.4 MB/sec     1.00      5.4±0.16ms     7.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.23  1401.1±49.48µs    11.9 MB/sec     1.00  1142.2±48.26µs    14.6 MB/sec
linter/default-rules/numpy/globals.py      1.13    163.2±8.43µs    18.1 MB/sec     1.00    144.3±9.26µs    20.5 MB/sec
linter/default-rules/pydantic/types.py     1.16      2.8±0.10ms     9.1 MB/sec     1.00      2.4±0.10ms    10.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.16ms     4.2 MB/sec    1.03     10.0±0.12ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1855.9±21.45µs     9.0 MB/sec    1.02  1891.0±10.82µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    189.6±2.02µs    15.6 MB/sec    1.04    196.9±6.02µs    15.0 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.03ms     6.1 MB/sec    1.03      4.3±0.06ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.02     12.6±0.15ms     3.2 MB/sec    1.00     12.4±0.08ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.04ms     4.7 MB/sec    1.00      3.5±0.03ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.01   368.4±10.18µs     8.0 MB/sec    1.00    364.9±6.51µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.02      5.9±0.09ms     4.3 MB/sec    1.00      5.8±0.08ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.02      6.8±0.07ms     6.0 MB/sec    1.00      6.6±0.07ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1390.6±16.72µs    12.0 MB/sec    1.00  1372.2±11.62µs    12.1 MB/sec
linter/default-rules/numpy/globals.py      1.02    141.7±2.21µs    20.8 MB/sec    1.00    139.2±1.45µs    21.2 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.0±0.04ms     8.4 MB/sec    1.00      3.0±0.06ms     8.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-08-08 03:56_

What's the current sentiment on this change? I _think_ I'm currently in favor, though it is a meaningful deviation (on unformatted code; on Black-formatted code, the behavior should be the same).

---

_@MichaReiser approved on 2023-08-08 06:55_

I'm in favor because it is consistent with the `trailing_comments` formatting and helps keep pragma comments close to where they were placed in the source. 

We can reconsider whether we should expand the parent of trailing comments in general, but this would need to be its own discusison.

---

_Label `formatter` added by @MichaReiser on 2023-08-08 06:55_

---

_@konstin approved on 2023-08-08 12:33_

---

_Merged by @charliermarsh on 2023-08-08 12:45_

---

_Closed by @charliermarsh on 2023-08-08 12:45_

---

_Branch deleted on 2023-08-08 12:45_

---
