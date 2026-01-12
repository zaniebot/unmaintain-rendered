```yaml
number: 5550
title: "Add `Issue Links` section to PR template"
type: pull_request
state: closed
author: evanrittenhouse
labels: []
assignees: []
base: main
head: main
created_at: 2023-07-06T01:16:08Z
updated_at: 2023-07-14T03:40:39Z
url: https://github.com/astral-sh/ruff/pull/5550
synced_at: 2026-01-12T15:55:18Z
```

# Add `Issue Links` section to PR template

---

_@evanrittenhouse_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-07-06 01:42_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      9.1±0.50ms     4.5 MB/sec     1.02      9.2±0.37ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.13ms     8.0 MB/sec     1.06      2.2±0.11ms     7.6 MB/sec
formatter/numpy/globals.py                 1.00   239.5±17.78µs    12.3 MB/sec     1.04   249.8±17.40µs    11.8 MB/sec
formatter/pydantic/types.py                1.00      4.7±0.23ms     5.4 MB/sec     1.00      4.7±0.36ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.02     16.0±0.86ms     2.5 MB/sec     1.00     15.6±0.62ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.19ms     4.3 MB/sec     1.03      4.0±0.21ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.05   546.0±50.85µs     5.4 MB/sec     1.00   520.5±42.21µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.33ms     3.8 MB/sec     1.07      7.2±0.67ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      7.6±0.37ms     5.3 MB/sec     1.01      7.8±0.39ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1690.3±114.26µs     9.9 MB/sec    1.01  1703.4±68.10µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.01   204.1±14.28µs    14.5 MB/sec     1.00   201.2±10.11µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.18ms     7.1 MB/sec     1.00      3.6±0.20ms     7.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.20     11.5±0.06ms     3.5 MB/sec    1.00      9.6±0.09ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.14      2.5±0.03ms     6.8 MB/sec    1.00      2.1±0.03ms     7.7 MB/sec
formatter/numpy/globals.py                 1.08    251.2±2.31µs    11.7 MB/sec    1.00    231.8±7.41µs    12.7 MB/sec
formatter/pydantic/types.py                1.14      5.4±0.03ms     4.7 MB/sec    1.00      4.8±0.05ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.00     15.8±0.14ms     2.6 MB/sec    1.01     15.9±0.11ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.03ms     4.0 MB/sec    1.01      4.2±0.03ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    426.6±5.42µs     6.9 MB/sec    1.01    431.0±6.66µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.08ms     3.6 MB/sec    1.02      7.2±0.07ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.07ms     5.0 MB/sec    1.03      8.4±0.06ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1676.8±12.19µs     9.9 MB/sec    1.02  1718.6±16.72µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    183.4±1.65µs    16.1 MB/sec    1.01    184.9±3.13µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.9 MB/sec    1.02      3.7±0.04ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @evanrittenhouse on 2023-07-06 22:14_

Force-pushed to rerun checks now that the `mkdocs` SSH issue is fixed. Ready for review.

---

_Comment by @evanrittenhouse on 2023-07-10 01:38_

@charliermarsh Just wanted to bump this, gave you edit rights in case you want to modify it

---

_Comment by @charliermarsh on 2023-07-10 16:26_

@evanrittenhouse - Appreciate the PR but I think I'd rather avoid adding another section for this. We do have a note at the top suggesting that users include links to relevant issues, and in general folks seem to do a good job of this.

We could either leave it as-is, or change `Does this pull request include references to any relevant issues?` to `Does this pull request include references to any relevant issues? (E.g., "Closes <Issue>.")`.

---

_Comment by @charliermarsh on 2023-07-14 03:08_

Gonna close based on the above, but let me know if you feel differently.

---

_Closed by @charliermarsh on 2023-07-14 03:08_

---

_Comment by @evanrittenhouse on 2023-07-14 03:40_

Nah makes sense. I was gonna close it today but got pulled into something at work

---
