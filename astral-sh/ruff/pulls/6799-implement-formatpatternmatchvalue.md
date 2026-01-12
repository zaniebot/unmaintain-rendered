```yaml
number: 6799
title: "Implement `FormatPatternMatchValue`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/value
created_at: 2023-08-23T01:41:23Z
updated_at: 2023-08-23T14:25:42Z
url: https://github.com/astral-sh/ruff/pull/6799
synced_at: 2026-01-12T15:55:22Z
```

# Implement `FormatPatternMatchValue`

---

_@charliermarsh_

## Summary

This is effectively #6608, but with additional tests.

We aren't properly handling parenthesized patterns, but that needs to be dealt with separately as it's somewhat involved.

Closes #6555


---

_Label `formatter` added by @charliermarsh on 2023-08-23 01:41_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-23 01:41_

---

_Review requested from @dhruvmanila by @charliermarsh on 2023-08-23 01:41_

---

_Comment by @github-actions[bot] on 2023-08-23 02:34_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.11      4.6±0.33ms     8.9 MB/sec     1.00      4.1±0.18ms     9.8 MB/sec
formatter/numpy/ctypeslib.py               1.00   903.6±29.21µs    18.4 MB/sec     1.00   904.3±48.37µs    18.4 MB/sec
formatter/numpy/globals.py                 1.00     92.6±5.07µs    31.9 MB/sec     1.04     96.2±7.78µs    30.7 MB/sec
formatter/pydantic/types.py                1.04  1845.3±158.22µs    13.8 MB/sec    1.00  1774.4±71.10µs    14.4 MB/sec
linter/all-rules/large/dataset.py          1.00     14.4±0.55ms     2.8 MB/sec     1.02     14.7±0.52ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.15ms     4.4 MB/sec     1.02      3.9±0.16ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.01   554.2±27.89µs     5.3 MB/sec     1.00   548.5±26.61µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.04      7.6±0.32ms     3.4 MB/sec     1.00      7.3±0.24ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.27ms     5.4 MB/sec     1.01      7.5±0.28ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1606.5±70.52µs    10.4 MB/sec     1.04  1678.7±73.66µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   203.3±10.89µs    14.5 MB/sec     1.03   208.4±16.81µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.05      3.6±0.20ms     7.1 MB/sec     1.00      3.4±0.14ms     7.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.11      5.4±0.35ms     7.5 MB/sec    1.00      4.9±0.34ms     8.4 MB/sec
formatter/numpy/ctypeslib.py               1.11  1098.3±70.43µs    15.2 MB/sec    1.00   990.2±58.52µs    16.8 MB/sec
formatter/numpy/globals.py                 1.08    107.5±9.34µs    27.4 MB/sec    1.00     99.7±6.68µs    29.6 MB/sec
formatter/pydantic/types.py                1.09      2.2±0.11ms    11.6 MB/sec    1.00      2.0±0.10ms    12.8 MB/sec
linter/all-rules/large/dataset.py          1.00     18.7±0.50ms     2.2 MB/sec    1.04     19.3±0.95ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.0±0.25ms     3.3 MB/sec    1.00      5.0±0.25ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   630.6±47.48µs     4.7 MB/sec    1.03   647.9±41.48µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.5±0.34ms     2.7 MB/sec    1.05      9.9±0.51ms     2.6 MB/sec
linter/default-rules/large/dataset.py      1.00     10.4±0.64ms     3.9 MB/sec    1.04     10.9±0.55ms     3.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.15ms     7.7 MB/sec    1.03      2.2±0.15ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   257.9±15.72µs    11.4 MB/sec    1.06   272.6±15.68µs    10.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.20ms     5.6 MB/sec    1.05      4.8±0.18ms     5.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/tests/snapshots/format@statement__match.py.snap`:513 on 2023-08-23 04:01_

What would the formatting be like after the fix for double parentheses? Would it still keep the parentheses from the original code i.e., format is as `(1)`?

---

_@dhruvmanila approved on 2023-08-23 04:45_

---

_@dhruvmanila reviewed on 2023-08-23 04:46_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/tests/snapshots/format@statement__match.py.snap`:513 on 2023-08-23 04:46_

Ah nevermind, it's answered in your other PR.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/pattern/pattern_match_value.rs`:4 on 2023-08-23 06:30_

Nit: `use crate::prelude::*`. It should allow removing `Buffer`, `FormatResult`, `PyFormatter`, and `AsFormat` (and it makes it easier to change the code in the future without having to add a ton of imports)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/pattern/pattern_match_value.rs`:14 on 2023-08-23 06:30_

```suggestion
        value.format().fmt(f)
```

---

_@MichaReiser approved on 2023-08-23 06:31_

Nice, this will make testing of other match patterns much easier.

I'm not sure if you saw this, but @evanrittenhouse was working on this https://github.com/astral-sh/ruff/pull/6608



---

_Review comment by @konstin on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/match.py`:245 on 2023-08-23 08:47_

Could you add a double parentheses test case?
```suggestion
    case (1):
        y = 1
    case (("a")):
        y = 1
```

---

_@konstin approved on 2023-08-23 08:49_

I added `Closes #6555` to the description

---

_Comment by @charliermarsh on 2023-08-23 13:36_

Thanks! I did see the other PR but I wanted to push these along so that I could start working on some of the other problems around comments and parentheses without so many `NOT_IMPLEMENTED` nodes in the test. I'd hoped to just update and merge that PR but I didn't have write permissions.

---

_Merged by @charliermarsh on 2023-08-23 14:01_

---

_Closed by @charliermarsh on 2023-08-23 14:01_

---

_Branch deleted on 2023-08-23 14:01_

---
