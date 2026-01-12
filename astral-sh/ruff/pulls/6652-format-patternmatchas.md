```yaml
number: 6652
title: "Format `PatternMatchAs`"
type: pull_request
state: merged
author: lkh42t
labels:
  - formatter
assignees: []
merged: true
base: main
head: format-pattern-match-as
created_at: 2023-08-17T14:31:32Z
updated_at: 2023-11-23T09:15:23Z
url: https://github.com/astral-sh/ruff/pull/6652
synced_at: 2026-01-12T15:55:22Z
```

# Format `PatternMatchAs`

---

_@lkh42t_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Add formatting for `PatternMatchAs`.

This closes #6641.

## Test Plan

Add tests for comments.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/pattern/pattern_match_as.rs`:31 on 2023-08-17 14:46_

Nit: We can move the `name` out of the branching
```suggestion
                    ]
                )?;
            } 
            name.format().fmt(f)?;
```

---

_@MichaReiser reviewed on 2023-08-17 14:47_

Thank you. 

I'm not sure how well we can test this just now with most of the pattern formatting missing, but could we add a few test cases that have comments between the pattern and the name?

---

_Comment by @github-actions[bot] on 2023-08-17 15:07_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.4±0.01ms    12.0 MB/sec    1.00      3.4±0.01ms    12.0 MB/sec
formatter/numpy/ctypeslib.py               1.00    716.4±4.28µs    23.2 MB/sec    1.00    714.0±3.59µs    23.3 MB/sec
formatter/numpy/globals.py                 1.00     76.6±0.29µs    38.5 MB/sec    1.01     77.3±0.96µs    38.2 MB/sec
formatter/pydantic/types.py                1.01  1395.5±22.66µs    18.3 MB/sec    1.00  1381.1±10.64µs    18.5 MB/sec
linter/all-rules/large/dataset.py          1.00     10.1±0.04ms     4.0 MB/sec    1.01     10.2±0.04ms     4.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.7±0.01ms     6.1 MB/sec    1.01      2.8±0.05ms     6.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    384.0±1.95µs     7.7 MB/sec    1.02    390.0±0.82µs     7.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.3±0.03ms     4.8 MB/sec    1.01      5.4±0.04ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.00      5.4±0.01ms     7.6 MB/sec    1.02      5.5±0.03ms     7.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1194.5±4.41µs    13.9 MB/sec    1.01   1205.8±1.27µs    13.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    137.5±0.61µs    21.5 MB/sec    1.01    139.4±0.75µs    21.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.02ms    10.4 MB/sec    1.00      2.5±0.02ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      5.0±0.28ms     8.2 MB/sec    1.01      5.0±0.31ms     8.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   957.8±45.85µs    17.4 MB/sec    1.02   979.6±46.61µs    17.0 MB/sec
formatter/numpy/globals.py                 1.00     97.3±6.15µs    30.3 MB/sec    1.07   104.6±11.36µs    28.2 MB/sec
formatter/pydantic/types.py                1.00  1936.1±80.39µs    13.2 MB/sec    1.03  1999.6±129.81µs    12.8 MB/sec
linter/all-rules/large/dataset.py          1.00     16.5±0.57ms     2.5 MB/sec    1.17     19.3±0.58ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.9±0.22ms     3.4 MB/sec    1.05      5.1±0.25ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   605.7±29.02µs     4.9 MB/sec    1.01   613.3±23.00µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.7±0.47ms     2.6 MB/sec    1.05     10.2±0.43ms     2.5 MB/sec
linter/default-rules/large/dataset.py      1.00     10.9±0.45ms     3.7 MB/sec    1.02     11.1±0.87ms     3.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.2±0.08ms     7.6 MB/sec    1.00      2.2±0.11ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   266.2±15.87µs    11.1 MB/sec    1.00   267.3±15.45µs    11.0 MB/sec
linter/default-rules/pydantic/types.py     1.05      4.8±0.44ms     5.3 MB/sec    1.00      4.6±0.20ms     5.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Converted to draft by @lkh42t on 2023-08-17 16:43_

---

_Marked ready for review by @lkh42t on 2023-08-19 09:30_

---

_Label `formatter` added by @konstin on 2023-08-21 08:32_

---

_@konstin approved on 2023-08-21 15:51_

---

_@dhruvmanila reviewed on 2023-08-22 03:47_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/tests/snapshots/format@statement__match.py.snap`:345 on 2023-08-22 03:47_

This might need to be handled in a different way as there seems to be 2 entries created for `PatternMatchAs` node comments where the first entry contains 1, 6, 7 and the second contains 2, 3, 4, 5.

```rust
{
    Node {
        kind: PatternMatchAs,
        range: 55..120,
        source: `pattern  # 2⏎`,
    }: {
        "leading": [
            SourceComment {
                text: "# 1",
                position: OwnLine,
                formatted: true,
            },
        ],
        "dangling": [],
        "trailing": [
            SourceComment {
                text: "# 6",
                position: EndOfLine,
                formatted: true,
            },
            SourceComment {
                text: "# 7",
                position: OwnLine,
                formatted: true,
            },
        ],
    },
    Node {
        kind: PatternMatchAs,
        range: 55..62,
        source: `pattern`,
    }: {
        "leading": [],
        "dangling": [],
        "trailing": [
            SourceComment {
                text: "# 2",
                position: EndOfLine,
                formatted: true,
            },
            SourceComment {
                text: "# 3",
                position: OwnLine,
                formatted: true,
            },
            SourceComment {
                text: "# 4",
                position: EndOfLine,
                formatted: true,
            },
            SourceComment {
                text: "# 5",
                position: OwnLine,
                formatted: true,
            },
        ],
    },
}
```

---

_@MichaReiser reviewed on 2023-08-22 06:45_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/pattern/pattern_match_as.rs`:29 on 2023-08-22 06:45_

I expect that Black's logic is to preserve parentheses around nested patterns. But we can track this in a separate issue

---

_@MichaReiser reviewed on 2023-08-22 06:53_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@statement__match.py.snap`:345 on 2023-08-22 06:53_

We'll need a custom handler in placement.rs for `MatchAs` that assigns all comments that come after the start of the name to the `MatchAs` clause.

Can you add a test case with a comment between the `as` and the name

```python
match pattern_comments:
    case (

        pattern 
        # 1
		as # 2
        # 3
		name #4
        # 5
    ):
        pass

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-22 22:42_

---

_Merged by @charliermarsh on 2023-08-22 23:58_

---

_Closed by @charliermarsh on 2023-08-22 23:58_

---

_Branch deleted on 2023-11-23 09:15_

---
