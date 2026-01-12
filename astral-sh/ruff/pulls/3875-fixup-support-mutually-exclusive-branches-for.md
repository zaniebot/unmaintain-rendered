```yaml
number: 3875
title: "fixup! Support mutually exclusive branches for `B031` (#3844)"
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: main
head: fixup/76e111c8
created_at: 2023-04-04T03:09:26Z
updated_at: 2023-04-04T03:37:36Z
url: https://github.com/astral-sh/ruff/pull/3875
synced_at: 2026-01-12T04:28:19Z
```

# fixup! Support mutually exclusive branches for `B031` (#3844)

---

_Pull request opened by @dhruvmanila on 2023-04-04 03:09_

I'm not sure why I added that condition, but nevertheless here's the fixup for that 🤔

---

_Comment by @github-actions[bot] on 2023-04-04 03:21_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.1±0.04ms     2.9 MB/sec    1.00     14.0±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    455.2±1.46µs     6.5 MB/sec    1.00    453.7±0.91µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.1±0.12ms     4.2 MB/sec    1.00      6.0±0.03ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.02      7.3±0.08ms     5.6 MB/sec    1.00      7.1±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1615.6±1.50µs    10.3 MB/sec    1.00  1615.9±10.30µs    10.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    178.4±0.40µs    16.5 MB/sec    1.01    179.9±0.63µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.00ms     7.7 MB/sec    1.00      3.3±0.01ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     17.3±0.36ms     2.4 MB/sec    1.00     16.9±0.23ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.5±0.16ms     3.7 MB/sec    1.00      4.4±0.09ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.01   515.8±16.09µs     5.7 MB/sec    1.00   511.5±23.28µs     5.8 MB/sec
linter/all-rules/pydantic/types.py         1.04      7.4±0.18ms     3.4 MB/sec    1.00      7.2±0.14ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.7±0.11ms     4.7 MB/sec    1.01      8.7±0.19ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1879.8±30.44µs     8.9 MB/sec    1.00  1888.7±35.41µs     8.8 MB/sec
linter/default-rules/numpy/globals.py      1.03    206.9±8.39µs    14.3 MB/sec    1.00    200.0±8.16µs    14.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.05ms     6.5 MB/sec    1.01      4.0±0.08ms     6.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-04-04 03:26_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_bugbear/rules/reuse_of_groupby_generator.rs`:158 on 2023-04-04 03:26_

(Did this part I tweaked here make sense? This was two conditions but I changed it to a `first().map_or()`.)

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/flake8_bugbear/rules/reuse_of_groupby_generator.rs`:158 on 2023-04-04 03:31_

Yes, it does. Your changes were a simplification of my two conditions (not empty and not `if` node). Here, `first` will return `None` if empty which means `map_or` will return the default value `false` which is correct.

---

_@dhruvmanila reviewed on 2023-04-04 03:31_

---

_@dhruvmanila reviewed on 2023-04-04 03:33_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/flake8_bugbear/rules/reuse_of_groupby_generator.rs`:158 on 2023-04-04 03:33_

We basically want to know if this node has an `else` block. It could come from an `if` or `elif` node. The bug was that it restricted to only an `elif` node which is proven by the added test case.

---

_Merged by @charliermarsh on 2023-04-04 03:34_

---

_Closed by @charliermarsh on 2023-04-04 03:34_

---

_Branch deleted on 2023-04-04 03:35_

---
