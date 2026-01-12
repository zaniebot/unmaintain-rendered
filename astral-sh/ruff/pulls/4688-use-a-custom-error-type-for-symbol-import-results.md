```yaml
number: 4688
title: Use a custom error type for symbol-import results
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/error-type
created_at: 2023-05-27T20:58:08Z
updated_at: 2023-05-30T13:19:33Z
url: https://github.com/astral-sh/ruff/pull/4688
synced_at: 2026-01-12T03:50:03Z
```

# Use a custom error type for symbol-import results

---

_Pull request opened by @charliermarsh on 2023-05-27 20:58_

## Summary

Follow-up to a previous PR -- see https://github.com/charliermarsh/ruff/pull/4649#discussion_r1205034552 for context.

---

_Comment by @github-actions[bot] on 2023-05-27 21:07_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     18.7±0.41ms     2.2 MB/sec    1.00     18.3±0.44ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.5±0.10ms     3.7 MB/sec    1.00      4.4±0.13ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.02   564.5±18.26µs     5.2 MB/sec    1.00   551.1±18.92µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.03      7.8±0.15ms     3.3 MB/sec    1.00      7.6±0.19ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.9±0.17ms     4.6 MB/sec    1.01      9.0±0.45ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1957.3±36.62µs     8.5 MB/sec    1.00  1913.8±58.28µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.07   242.8±17.15µs    12.2 MB/sec    1.00   226.5±10.09µs    13.0 MB/sec
linter/default-rules/pydantic/types.py     1.03      4.1±0.27ms     6.2 MB/sec    1.00      4.0±0.11ms     6.4 MB/sec
parser/large/dataset.py                    1.00      6.8±0.14ms     6.0 MB/sec    1.00      6.8±0.21ms     6.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1382.7±34.40µs    12.0 MB/sec    1.02  1410.2±103.73µs    11.8 MB/sec
parser/numpy/globals.py                    1.02    135.9±4.54µs    21.7 MB/sec    1.00    133.9±8.35µs    22.0 MB/sec
parser/pydantic/types.py                   1.00      3.1±0.07ms     8.3 MB/sec    1.00      3.1±0.23ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.04     18.7±0.48ms     2.2 MB/sec    1.00     18.0±0.40ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.6±0.13ms     3.7 MB/sec    1.00      4.5±0.21ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   538.5±30.70µs     5.5 MB/sec    1.01   541.2±29.11µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.8±0.20ms     3.3 MB/sec    1.00      7.7±0.23ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.04      9.3±0.19ms     4.4 MB/sec    1.00      9.0±0.33ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04  1957.1±54.97µs     8.5 MB/sec    1.00  1886.1±100.84µs     8.8 MB/sec
linter/default-rules/numpy/globals.py      1.05   220.9±12.47µs    13.4 MB/sec    1.00    211.4±7.40µs    14.0 MB/sec
linter/default-rules/pydantic/types.py     1.05      4.1±0.08ms     6.2 MB/sec    1.00      3.9±0.11ms     6.5 MB/sec
parser/large/dataset.py                    1.00      6.6±0.14ms     6.2 MB/sec    1.00      6.6±0.12ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1252.0±29.47µs    13.3 MB/sec    1.00  1255.2±25.69µs    13.3 MB/sec
parser/numpy/globals.py                    1.00    129.0±6.80µs    22.9 MB/sec    1.00    128.4±4.52µs    23.0 MB/sec
parser/pydantic/types.py                   1.01      2.9±0.10ms     8.9 MB/sec    1.00      2.8±0.07ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-05-27 21:08_

---

_Review comment by @charliermarsh on `crates/ruff/src/importer/mod.rs`:241 on 2023-05-27 21:08_

This feels a bit off, like this should probably be called `GetResult`, but it's not an actual `Result`. I could instead make this `GetError`, and have `get_symbol` return `Option<Result<(Edit, String), GetError>> `?

---

_Review comment by @MichaReiser on `crates/ruff/src/importer/mod.rs`:241 on 2023-05-27 21:36_

Agree. That feels odd. Why do we need it when adding a custom error type?

---

_@MichaReiser reviewed on 2023-05-27 21:36_

---

_Review comment by @charliermarsh on `crates/ruff/src/importer/mod.rs`:241 on 2023-05-28 03:29_

Fixed.

---

_@charliermarsh reviewed on 2023-05-28 03:29_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-28 03:29_

---

_@MichaReiser approved on 2023-05-30 06:33_

---

_Merged by @charliermarsh on 2023-05-30 13:19_

---

_Closed by @charliermarsh on 2023-05-30 13:19_

---

_Branch deleted on 2023-05-30 13:19_

---
