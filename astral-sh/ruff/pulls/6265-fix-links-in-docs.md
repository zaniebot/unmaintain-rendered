```yaml
number: 6265
title: Fix links in docs
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: fix-link-hack
created_at: 2023-08-02T07:01:12Z
updated_at: 2023-08-02T07:46:24Z
url: https://github.com/astral-sh/ruff/pull/6265
synced_at: 2026-01-12T02:58:30Z
```

# Fix links in docs

---

_Pull request opened by @harupy on 2023-08-02 07:01_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Before:

<img width="1031" alt="Screen Shot 2023-08-02 at 15 57 10" src="https://github.com/astral-sh/ruff/assets/17039389/171a21d5-01a5-4aa5-8079-4e7f8a59ade8">

After:

<img width="1031" alt="Screen Shot 2023-08-02 at 15 58 03" src="https://github.com/astral-sh/ruff/assets/17039389/afd1b9b7-89e0-4e38-a4a6-e3255b62f021">


## Test Plan

<!-- How was it tested? -->

Manual inspection


---

_Review comment by @harupy on `crates/ruff_dev/src/generate_docs.rs`:84 on 2023-08-02 07:12_

Before:

```md
- [`pytest` documentation: `pytest.raises`][pytest` documentation: `pytest.raises](https://docs.pytest.org/en/latest/reference/reference.html#pytest-raises)
```

After:

```md
- [`pytest` documentation: `pytest.raises`](https://docs.pytest.org/en/latest/reference/reference.html#pytest-raises)
```

---

_@harupy reviewed on 2023-08-02 07:12_

---

_Comment by @github-actions[bot] on 2023-08-02 07:29_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      9.2±0.55ms     4.4 MB/sec     1.02      9.3±0.54ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00  1796.1±110.18µs     9.3 MB/sec    1.03  1842.1±114.30µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00   205.6±13.40µs    14.4 MB/sec     1.08   222.4±16.96µs    13.3 MB/sec
formatter/pydantic/types.py                1.03      4.1±0.17ms     6.2 MB/sec     1.00      4.0±0.22ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     13.0±0.78ms     3.1 MB/sec     1.00     13.0±0.57ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.14ms     5.2 MB/sec     1.05      3.4±0.14ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.02   464.7±35.07µs     6.3 MB/sec     1.00   456.2±29.77µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.7±0.25ms     4.4 MB/sec     1.04      6.0±0.36ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.02      6.8±0.29ms     6.0 MB/sec     1.00      6.6±0.25ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1426.5±68.15µs    11.7 MB/sec     1.00  1388.2±68.08µs    12.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    160.9±9.41µs    18.3 MB/sec     1.01   161.8±10.67µs    18.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.12ms     8.7 MB/sec     1.00      2.9±0.17ms     8.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.9±0.14ms     4.1 MB/sec    1.01     10.0±0.17ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1928.0±25.87µs     8.6 MB/sec    1.00  1920.8±27.57µs     8.7 MB/sec
formatter/numpy/globals.py                 1.00    212.0±4.93µs    13.9 MB/sec    1.01    214.0±7.00µs    13.8 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.09ms     6.0 MB/sec    1.01      4.3±0.09ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.17ms     3.0 MB/sec    1.00     13.6±0.22ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.05ms     4.7 MB/sec    1.00      3.5±0.04ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    431.4±6.14µs     6.8 MB/sec    1.00    432.9±6.49µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.09ms     4.2 MB/sec    1.01      6.1±0.09ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.4±0.16ms     5.5 MB/sec    1.00      7.4±0.07ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1502.8±24.00µs    11.1 MB/sec    1.00  1483.1±24.66µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.01    168.8±5.22µs    17.5 MB/sec    1.00    167.5±3.31µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.05ms     7.9 MB/sec    1.00      3.2±0.04ms     7.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin approved on 2023-08-02 07:42_

---

_Merged by @konstin on 2023-08-02 07:42_

---

_Closed by @konstin on 2023-08-02 07:42_

---

_Branch deleted on 2023-08-02 07:46_

---
