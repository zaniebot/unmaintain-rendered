```yaml
number: 5853
title: "Move a few more candidate rules to the deferred `Binding`-only pass"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/convention
created_at: 2023-07-18T04:13:24Z
updated_at: 2023-07-19T01:20:23Z
url: https://github.com/astral-sh/ruff/pull/5853
synced_at: 2026-01-12T03:30:22Z
```

# Move a few more candidate rules to the deferred `Binding`-only pass

---

_Pull request opened by @charliermarsh on 2023-07-18 04:13_

## Summary

No behavior change, but this is in theory more efficient, since we can just iterate over the flat `Binding` vector rather than having to iterate over binding chains via the `Scope`.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-18 04:13_

---

_Comment by @github-actions[bot] on 2023-07-18 04:30_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.4±0.04ms     4.3 MB/sec    1.00      9.4±0.04ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1859.5±3.30µs     9.0 MB/sec    1.00   1857.4±3.28µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    207.7±0.25µs    14.2 MB/sec    1.01    209.7±5.41µs    14.1 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.01ms     6.4 MB/sec    1.00      4.0±0.01ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     13.5±0.07ms     3.0 MB/sec    1.05     14.2±0.12ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.02      3.4±0.01ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    437.0±1.93µs     6.8 MB/sec    1.01    443.0±1.33µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.03ms     4.2 MB/sec    1.04      6.2±0.04ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.02ms     5.8 MB/sec    1.07      7.5±0.03ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1476.1±2.04µs    11.3 MB/sec    1.05  1553.9±10.17µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    161.2±0.20µs    18.3 MB/sec    1.04    167.0±1.69µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.2 MB/sec    1.07      3.3±0.01ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.05     11.5±0.36ms     3.5 MB/sec    1.00     10.9±0.06ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.09      2.3±0.13ms     7.3 MB/sec    1.00      2.1±0.02ms     7.9 MB/sec
formatter/numpy/globals.py                 1.06    246.0±9.12µs    12.0 MB/sec    1.00    231.9±7.32µs    12.7 MB/sec
formatter/pydantic/types.py                1.08      5.1±0.26ms     5.0 MB/sec    1.00      4.7±0.04ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.09     16.5±0.56ms     2.5 MB/sec    1.00     15.1±0.04ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.09      4.4±0.10ms     3.8 MB/sec    1.00      4.0±0.02ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.27   529.4±29.52µs     5.6 MB/sec    1.00    416.4±5.37µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.13      7.8±0.44ms     3.3 MB/sec    1.00      6.9±0.03ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.12      9.0±0.51ms     4.5 MB/sec    1.00      8.0±0.02ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.14  1854.2±72.63µs     9.0 MB/sec    1.00   1630.4±8.74µs    10.2 MB/sec
linter/default-rules/numpy/globals.py      1.18    205.6±9.73µs    14.4 MB/sec    1.00    174.6±0.90µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.11      4.0±0.18ms     6.4 MB/sec    1.00      3.6±0.02ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@MichaReiser approved on 2023-07-18 06:25_

Looks good (hard for me to review, as I'm not too familiar with the semantic model but I trust you on this). Could you run the cpython benchmarks and post your results. 

As noted on the other PR. I'm still confused by the term *phase*. Or better, what's considered a phase and what is not. Why is `check_bindings` considered a phase? Creating the bindings is a phase, but checking them runs after binding... I think it would help to clarify the concepts, it could be a sign that we lack concepts or have the wrong concepts.

---

_Renamed from "Move a few more candidate rules to the `Binding`-only phase" to "Move a few more candidate rules to the deferred `Binding`-only pass" by @charliermarsh on 2023-07-19 00:32_

---

_Merged by @charliermarsh on 2023-07-19 00:59_

---

_Closed by @charliermarsh on 2023-07-19 00:59_

---

_Branch deleted on 2023-07-19 00:59_

---
