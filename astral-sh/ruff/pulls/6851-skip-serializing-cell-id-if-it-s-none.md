```yaml
number: 6851
title: "Skip serializing cell ID if it's None"
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: fix-null-id
created_at: 2023-08-24T12:12:14Z
updated_at: 2023-08-24T13:45:16Z
url: https://github.com/astral-sh/ruff/pull/6851
synced_at: 2026-01-12T02:45:38Z
```

# Skip serializing cell ID if it's None

---

_Pull request opened by @harupy on 2023-08-24 12:12_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fix #6834 

## Test Plan

<!-- How was it tested? -->

Need tests?

---

_Review requested from @dhruvmanila by @harupy on 2023-08-24 12:12_

---

_Renamed from "Skip serializing cell ID if it's None" to "Skip serializing cell IDs if it's None" by @harupy on 2023-08-24 12:12_

---

_Renamed from "Skip serializing cell IDs if it's None" to "Skip serializing cell ID if it's None" by @harupy on 2023-08-24 12:12_

---

_@konstin reviewed on 2023-08-24 12:21_

Could you please add either a <4.5 notebook or reuse crates/ruff/resources/test/fixtures/jupyter/valid.ipynb to add a test in notebook.rs? I think the easiest is to serialize it to a string and check that the string doesn't contain `"id"`. (validating against the jupyter notebook schema would also be possible but it feels like an overkill)

---

_Comment by @github-actions[bot] on 2023-08-24 12:29_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.15      4.1±0.04ms    10.0 MB/sec    1.00      3.6±0.04ms    11.4 MB/sec
formatter/numpy/ctypeslib.py               1.15    821.8±5.66µs    20.3 MB/sec    1.00   714.4±12.30µs    23.3 MB/sec
formatter/numpy/globals.py                 1.08     80.2±0.31µs    36.8 MB/sec    1.00     74.5±0.41µs    39.6 MB/sec
formatter/pydantic/types.py                1.15  1640.9±13.51µs    15.5 MB/sec    1.00  1429.7±21.87µs    17.8 MB/sec
linter/all-rules/large/dataset.py          1.00     10.4±0.02ms     3.9 MB/sec    1.03     10.6±0.13ms     3.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.9±0.00ms     5.8 MB/sec    1.01      2.9±0.00ms     5.8 MB/sec
linter/all-rules/numpy/globals.py          1.01    322.7±0.97µs     9.1 MB/sec    1.00    319.9±1.71µs     9.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.4±0.02ms     4.7 MB/sec    1.01      5.5±0.02ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.00      5.5±0.03ms     7.4 MB/sec    1.04      5.7±0.02ms     7.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1179.4±2.82µs    14.1 MB/sec    1.02   1201.3±3.46µs    13.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    123.6±1.56µs    23.9 MB/sec    1.02    125.9±0.27µs    23.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.02ms    10.2 MB/sec    1.02      2.6±0.01ms    10.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.12      5.3±0.23ms     7.7 MB/sec    1.00      4.7±0.18ms     8.6 MB/sec
formatter/numpy/ctypeslib.py               1.19  1114.9±45.68µs    14.9 MB/sec    1.00   939.6±36.81µs    17.7 MB/sec
formatter/numpy/globals.py                 1.09    108.9±5.21µs    27.1 MB/sec    1.00    100.2±7.34µs    29.5 MB/sec
formatter/pydantic/types.py                1.14      2.2±0.13ms    11.5 MB/sec    1.00  1945.8±112.11µs    13.1 MB/sec
linter/all-rules/large/dataset.py          1.00     14.3±0.46ms     2.8 MB/sec    1.01     14.4±0.40ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.13ms     4.2 MB/sec    1.00      4.0±0.18ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   492.2±19.96µs     6.0 MB/sec    1.04   512.3±50.79µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.4±0.26ms     3.4 MB/sec    1.01      7.5±0.32ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.23ms     5.0 MB/sec    1.01      8.1±0.27ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1709.1±59.40µs     9.7 MB/sec    1.02  1744.2±75.39µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00   200.9±12.28µs    14.7 MB/sec    1.03   207.6±15.14µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.7±0.11ms     6.9 MB/sec    1.00      3.6±0.13ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @harupy on 2023-08-24 12:46_

@konstin Added a test!

---

_Review comment by @dhruvmanila on `crates/ruff/src/jupyter/schema.rs`:153 on 2023-08-24 12:51_

Wait, we already have `#[skip_serializing_none]` for other cell types, it's not added to `CodeCell` because the field `execution_count` is [required](https://github.com/jupyter/nbformat/blob/a3e787dccbf629c8cbae46f79e74d53ded8b53a9/nbformat/v4/nbformat.v4.5.schema.json#L197). The solution is to use `serialize_always` _only_ on `id` field of `CodeCell`.

```suggestion
    #[serialize_always]
```

---

_Review comment by @dhruvmanila on `crates/ruff/src/jupyter/schema.rs`:123 on 2023-08-24 12:51_

We already use `#[skip_serializing_none]` on the struct

```suggestion
```

---

_Review comment by @dhruvmanila on `crates/ruff/src/jupyter/schema.rs`:139 on 2023-08-24 12:51_

We already use `#[skip_serializing_none]` on the struct

```suggestion
```

---

_@dhruvmanila approved on 2023-08-24 12:51_

Thanks! Can you make the suggested changes and then it should be good to go!

---

_@dhruvmanila reviewed on 2023-08-24 13:07_

---

_Review comment by @dhruvmanila on `crates/ruff/src/jupyter/schema.rs`:153 on 2023-08-24 13:07_

Ah, my bad. Here, I think it's fine to use `#[serde(...)]`

---

_@konstin approved on 2023-08-24 13:13_

thanks

---

_Merged by @konstin on 2023-08-24 13:20_

---

_Closed by @konstin on 2023-08-24 13:20_

---

_Branch deleted on 2023-08-24 13:20_

---
