```yaml
number: 3824
title: "Extend `unncessary-generator-any-all` to set comprehensions"
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/set
created_at: 2023-03-30T18:39:00Z
updated_at: 2023-03-31T16:45:52Z
url: https://github.com/astral-sh/ruff/pull/3824
synced_at: 2026-01-12T04:28:19Z
```

# Extend `unncessary-generator-any-all` to set comprehensions

---

_Pull request opened by @charliermarsh on 2023-03-30 18:39_

_No description provided._

---

_Label `rule` added by @charliermarsh on 2023-03-30 18:39_

---

_Comment by @github-actions[bot] on 2023-03-30 18:51_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     19.2±0.48ms     2.1 MB/sec    1.00     18.6±0.38ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.7±0.17ms     3.5 MB/sec    1.00      4.6±0.11ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.03   618.3±16.28µs     4.8 MB/sec    1.00   599.7±15.80µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.0±0.24ms     3.2 MB/sec    1.00      7.9±0.22ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.04      9.8±0.72ms     4.2 MB/sec    1.00      9.4±0.18ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.1±0.06ms     7.9 MB/sec    1.00      2.1±0.05ms     7.9 MB/sec
linter/default-rules/numpy/globals.py      1.06    260.0±9.70µs    11.3 MB/sec    1.00    246.1±8.21µs    12.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.4±0.13ms     5.7 MB/sec    1.00      4.4±0.18ms     5.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     18.5±0.70ms     2.2 MB/sec    1.07     19.9±0.74ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.9±0.24ms     3.4 MB/sec    1.06      5.1±0.22ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   587.3±25.64µs     5.0 MB/sec    1.06   621.5±29.82µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.0±0.30ms     3.2 MB/sec    1.11      8.8±0.60ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00      9.8±0.40ms     4.2 MB/sec    1.04     10.2±0.25ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.15ms     7.8 MB/sec    1.05      2.3±0.06ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   250.4±18.66µs    11.8 MB/sec    1.04   259.9±26.08µs    11.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.21ms     5.6 MB/sec    1.02      4.6±0.14ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_pie/rules.rs`:350 on 2023-03-31 06:17_

Nit
```suggestion
    if (matches!(id, "all" | "any") && args.len() == 1 {
```

---

_@MichaReiser approved on 2023-03-31 06:19_

I'll re-run the benchmark because it seems that the PR regressed performance a lot but I doubt this is really the case. Please check them before merging for any significant regressions before merging.

---

_Merged by @charliermarsh on 2023-03-31 16:29_

---

_Closed by @charliermarsh on 2023-03-31 16:29_

---

_Branch deleted on 2023-03-31 16:29_

---
