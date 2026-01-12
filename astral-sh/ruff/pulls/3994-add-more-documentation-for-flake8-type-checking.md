```yaml
number: 3994
title: Add more documentation for flake8-type-checking
type: pull_request
state: merged
author: tjkuson
labels:
  - documentation
assignees: []
merged: true
base: main
head: type_checking_docs
created_at: 2023-04-17T13:34:05Z
updated_at: 2023-04-17T14:00:48Z
url: https://github.com/astral-sh/ruff/pull/3994
synced_at: 2026-01-12T15:55:14Z
```

# Add more documentation for flake8-type-checking

---

_@tjkuson_

Add documentation to the flake8-type-checking rules that check for runtime imports inside type-checking blocks and empty type-checking blocks.

Builds on #3886 to complete the flake8-type-checking ruleset.

Related to #2646.

---

_Comment by @github-actions[bot] on 2023-04-17 13:46_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     19.4±0.56ms     2.1 MB/sec    1.00     18.9±0.67ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      4.7±0.45ms     3.5 MB/sec    1.00      4.5±0.13ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.01   615.3±26.03µs     4.8 MB/sec    1.00   606.8±22.00µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.03      8.3±0.24ms     3.1 MB/sec    1.00      8.0±0.25ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.05      9.7±0.40ms     4.2 MB/sec    1.00      9.2±0.37ms     4.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04      2.1±0.08ms     7.8 MB/sec    1.00      2.1±0.08ms     8.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    245.6±8.04µs    12.0 MB/sec    1.08   264.7±20.83µs    11.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.15ms     5.7 MB/sec    1.01      4.5±0.15ms     5.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     16.3±0.61ms     2.5 MB/sec    1.00     15.8±0.11ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.1±0.06ms     4.0 MB/sec    1.00      4.1±0.06ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    488.5±5.40µs     6.0 MB/sec    1.00    489.6±6.81µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.05ms     3.8 MB/sec    1.00      6.7±0.06ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.09ms     5.1 MB/sec    1.00      7.9±0.04ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1735.1±21.36µs     9.6 MB/sec    1.01  1748.5±19.17µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    189.2±3.41µs    15.6 MB/sec    1.01    192.0±5.37µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.1 MB/sec    1.01      3.6±0.04ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `documentation` added by @charliermarsh on 2023-04-17 13:51_

---

_Merged by @charliermarsh on 2023-04-17 13:51_

---

_Closed by @charliermarsh on 2023-04-17 13:51_

---

_Comment by @charliermarsh on 2023-04-17 13:52_

Thanks! Looks great.

---
