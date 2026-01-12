```yaml
number: 3654
title: "Avoid `RUF007` fixes for more than two arguments"
type: pull_request
state: merged
author: JonathanPlasse
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-ruf007-false-positive
created_at: 2023-03-21T19:34:11Z
updated_at: 2023-03-22T15:02:08Z
url: https://github.com/astral-sh/ruff/pull/3654
synced_at: 2026-01-12T15:55:13Z
```

# Avoid `RUF007` fixes for more than two arguments

---

_@JonathanPlasse_

- Close #3651

---

_@charliermarsh reviewed on 2023-03-21 19:36_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/pairwise_over_zipped.rs`:96 on 2023-03-21 19:36_

I think we may want to avoid _both_ `args != 2` _and_ `strict=True`, since the latter isn't supported?

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/pairwise_over_zipped.rs`:96 on 2023-03-21 19:37_

I think this current approach only catches extra arguments, but not the unsupported kwargs.

---

_@charliermarsh reviewed on 2023-03-21 19:37_

---

_Comment by @github-actions[bot] on 2023-03-21 20:03_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.5±0.07ms     3.0 MB/sec    1.00     13.4±0.04ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec    1.00      3.5±0.00ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    483.4±1.06µs     6.1 MB/sec    1.01    486.6±0.88µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.02ms     4.3 MB/sec    1.00      5.9±0.01ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.02ms     5.7 MB/sec    1.00      7.1±0.01ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1615.5±3.99µs    10.3 MB/sec    1.00   1603.5±6.07µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    178.5±0.47µs    16.5 MB/sec    1.00    177.6±0.68µs    16.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.4±0.01ms     7.6 MB/sec    1.00      3.4±0.01ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.0±0.09ms     2.7 MB/sec    1.00     14.9±0.06ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.01ms     4.1 MB/sec    1.00      4.0±0.03ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    438.7±4.07µs     6.7 MB/sec    1.00    438.5±8.39µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.03ms     3.9 MB/sec    1.00      6.6±0.03ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.02ms     5.0 MB/sec    1.00      8.1±0.04ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1732.9±13.68µs     9.6 MB/sec    1.00   1722.8±6.70µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    173.7±1.02µs    17.0 MB/sec    1.00    174.5±3.30µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.01ms     6.8 MB/sec    1.00      3.8±0.02ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/ruff/rules/pairwise_over_zipped.rs`:96 on 2023-03-21 20:54_

Done.

---

_@JonathanPlasse reviewed on 2023-03-21 20:54_

---

_Renamed from "Fix RUF007 false positive" to "Avoid `RUF007` fixes for more than two arguments" by @charliermarsh on 2023-03-21 22:12_

---

_Label `bug` added by @charliermarsh on 2023-03-21 22:12_

---

_Merged by @charliermarsh on 2023-03-21 22:17_

---

_Closed by @charliermarsh on 2023-03-21 22:17_

---

_Review comment by @aberres on `crates/ruff/src/rules/ruff/rules/pairwise_over_zipped.rs`:96 on 2023-03-22 08:12_

@charliermarsh  But isn't this a valid case for pairwise `zip(foo[:-1], foo[1:], strict=True)`?

---

_@aberres reviewed on 2023-03-22 08:12_

---

_@JonathanPlasse reviewed on 2023-03-22 08:28_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/ruff/rules/pairwise_over_zipped.rs`:96 on 2023-03-22 08:28_

Indeed

---

_Branch deleted on 2023-03-22 08:29_

---

_@charliermarsh reviewed on 2023-03-22 14:59_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/pairwise_over_zipped.rs`:96 on 2023-03-22 14:59_

Oh hmm, in that case, isn't _either_ `strict` argument acceptable? (`False` is of course fine.)

---

_Review comment by @aberres on `crates/ruff/src/rules/ruff/rules/pairwise_over_zipped.rs`:96 on 2023-03-22 15:02_

I'd say yes. 

---

_@aberres reviewed on 2023-03-22 15:02_

---
