```yaml
number: 5591
title: "Rework upstream categories so we can `all_rules()`"
type: pull_request
state: merged
author: akx
labels: []
assignees: []
merged: true
base: main
head: nopestream-categories
created_at: 2023-07-07T11:33:37Z
updated_at: 2023-07-10T13:54:11Z
url: https://github.com/astral-sh/ruff/pull/5591
synced_at: 2026-01-12T15:55:18Z
```

# Rework upstream categories so we can `all_rules()`

---

_@akx_

## Summary

This PR reworks the `upstream_categories` mechanism that is only used for documentation purposes to make it easier to generate docs using `all_rules()`. The new implementation also relies on "tribal knowledge" about rule codes, so it's not the best implementation, but gets us forward.

Another option would be to change the rule-defining proc macros to allow configuring an optional `RuleCategory`, but that seems more heavy-handed and possibly unnecessary in the long run...

Draft since this builds on #5439.

cc @charliermarsh :)

---

_Comment by @github-actions[bot] on 2023-07-07 11:46_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.03      8.1±0.03ms     5.0 MB/sec    1.00      7.9±0.04ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.03   1785.0±1.62µs     9.3 MB/sec    1.00   1740.2±2.40µs     9.6 MB/sec
formatter/numpy/globals.py                 1.02    198.4±0.31µs    14.9 MB/sec    1.00    195.0±0.57µs    15.1 MB/sec
formatter/pydantic/types.py                1.04      3.9±0.01ms     6.5 MB/sec    1.00      3.8±0.00ms     6.7 MB/sec
linter/all-rules/large/dataset.py          1.00     13.5±0.07ms     3.0 MB/sec    1.00     13.5±0.07ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.02ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    431.5±0.99µs     6.8 MB/sec    1.01    434.3±0.64µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.02ms     4.3 MB/sec    1.01      6.0±0.02ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.02ms     6.1 MB/sec    1.01      6.7±0.02ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1458.2±2.65µs    11.4 MB/sec    1.01   1471.9±3.35µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    166.7±0.22µs    17.7 MB/sec    1.01    168.4±0.24µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.01      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.4±0.08ms     4.3 MB/sec    1.00      9.4±0.19ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.1±0.03ms     8.1 MB/sec    1.00      2.1±0.03ms     8.1 MB/sec
formatter/numpy/globals.py                 1.00    231.2±3.52µs    12.8 MB/sec    1.02   236.4±10.72µs    12.5 MB/sec
formatter/pydantic/types.py                1.01      4.6±0.08ms     5.5 MB/sec    1.00      4.6±0.07ms     5.6 MB/sec
linter/all-rules/large/dataset.py          1.01     16.2±0.19ms     2.5 MB/sec    1.00     16.1±0.16ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.05ms     4.0 MB/sec    1.01      4.2±0.05ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   505.1±12.95µs     5.8 MB/sec    1.02    513.4±7.71µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.2±0.10ms     3.6 MB/sec    1.00      7.1±0.08ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.06ms     5.0 MB/sec    1.01      8.2±0.07ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1738.7±18.75µs     9.6 MB/sec    1.01  1764.3±22.81µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    204.3±2.80µs    14.4 MB/sec    1.02    208.2±5.21µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.06ms     7.0 MB/sec    1.01      3.7±0.03ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @konstin on `crates/ruff/src/registry.rs`:10 on 2023-07-07 11:54_

nit:
```suggestion
use crate::codes;
```
(i'm surprised clippy doesn't complain about this)

---

_Review comment by @konstin on `crates/ruff_dev/src/generate_rules_table.rs`:54 on 2023-07-07 11:55_

"and not active by default" or something similar, but i'll leave the exact phrasing up to @charliermarsh 

---

_@konstin approved on 2023-07-07 11:56_

---

_Review requested from @charliermarsh by @konstin on 2023-07-07 11:56_

---

_Review comment by @akx on `crates/ruff_dev/src/generate_rules_table.rs`:54 on 2023-07-07 12:40_

That can be discussed in #5439 (too) :)

---

_@akx reviewed on 2023-07-07 12:40_

---

_Comment by @charliermarsh on 2023-07-10 02:30_

@akx - Do you mind rebasing?

---

_Marked ready for review by @akx on 2023-07-10 07:57_

---

_Comment by @akx on 2023-07-10 07:57_

@charliermarsh Rebased 👍 

---

_Merged by @charliermarsh on 2023-07-10 13:41_

---

_Closed by @charliermarsh on 2023-07-10 13:41_

---

_Branch deleted on 2023-07-10 13:54_

---
