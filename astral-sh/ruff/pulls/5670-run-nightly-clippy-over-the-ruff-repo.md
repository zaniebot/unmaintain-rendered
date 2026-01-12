```yaml
number: 5670
title: Run nightly Clippy over the Ruff repo
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/clippy
created_at: 2023-07-11T03:25:17Z
updated_at: 2023-07-17T09:55:37Z
url: https://github.com/astral-sh/ruff/pull/5670
synced_at: 2026-01-12T15:55:19Z
```

# Run nightly Clippy over the Ruff repo

---

_@charliermarsh_

## Summary

This is the result of running `cargo +nightly clippy --workspace --all-targets --all-features -- -D warnings` and fixing all violations. Just wanted to see if there were any interesting new checks on nightly :eyes:

---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-11 03:25_

---

_Label `internal` added by @charliermarsh on 2023-07-11 03:25_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/isort/order.rs`:50 on 2023-07-11 03:25_

(Calling `.into_inter()` on a method that accepts `IntoIter`.)

---

_@charliermarsh reviewed on 2023-07-11 03:25_

---

_@zanieb approved on 2023-07-11 03:30_

Seems reasonable. It'd be cool to automate this but if it can't autofix most of the violations it seems tricky.

---

_Comment by @github-actions[bot] on 2023-07-11 03:35_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.9±0.01ms     5.1 MB/sec    1.01      8.0±0.03ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1814.1±2.19µs     9.2 MB/sec    1.00   1815.9±1.62µs     9.2 MB/sec
formatter/numpy/globals.py                 1.00    204.3±3.15µs    14.4 MB/sec    1.00    204.0±0.58µs    14.5 MB/sec
formatter/pydantic/types.py                1.01      4.0±0.00ms     6.4 MB/sec    1.00      3.9±0.02ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.00     13.5±0.06ms     3.0 MB/sec    1.01     13.6±0.04ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.01      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    435.7±0.77µs     6.8 MB/sec    1.00    437.6±0.68µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.02ms     4.2 MB/sec    1.01      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.03ms     6.0 MB/sec    1.03      6.9±0.01ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1491.9±3.70µs    11.2 MB/sec    1.02   1521.2±2.80µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    171.6±3.04µs    17.2 MB/sec    1.01    172.9±0.30µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.02ms     8.3 MB/sec    1.03      3.1±0.01ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.9±0.22ms     3.7 MB/sec    1.01     11.0±0.17ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.4±0.08ms     6.8 MB/sec    1.00      2.4±0.05ms     6.8 MB/sec
formatter/numpy/globals.py                 1.00    274.1±8.41µs    10.8 MB/sec    1.02   278.3±14.46µs    10.6 MB/sec
formatter/pydantic/types.py                1.00      5.3±0.10ms     4.8 MB/sec    1.02      5.5±0.10ms     4.7 MB/sec
linter/all-rules/large/dataset.py          1.04     19.4±1.09ms     2.1 MB/sec    1.00     18.7±0.72ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.07      5.3±0.40ms     3.2 MB/sec    1.00      4.9±0.06ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   606.4±21.17µs     4.9 MB/sec    1.00   605.0±23.78µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.03      9.0±0.62ms     2.8 MB/sec    1.00      8.8±0.46ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00      9.3±0.22ms     4.4 MB/sec    1.04      9.7±0.22ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1980.0±39.07µs     8.4 MB/sec    1.01  1997.6±41.77µs     8.3 MB/sec
linter/default-rules/numpy/globals.py      1.01    237.3±5.79µs    12.4 MB/sec    1.00    234.1±6.97µs    12.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.2±0.10ms     6.1 MB/sec    1.01      4.2±0.09ms     6.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-07-11 03:43_

Eventually these rules will make it out of nightly and into subsequent Rust releases, at which point we'll be able to incorporate them into our existing Clippy checks for free :)

---

_Merged by @charliermarsh on 2023-07-11 03:44_

---

_Closed by @charliermarsh on 2023-07-11 03:44_

---

_Branch deleted on 2023-07-11 03:44_

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/printer.rs`:190 on 2023-07-11 05:24_

Hmm, this will be somewhat annoying if we ever add instance fields. You then need to update all usages.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/generated.rs`:38 on 2023-07-11 05:29_

I wouldn't change the generated file. Some rules have fields and thus require using default. Having different usages for different rules will make it annoying to generate the code

Or is calling default explicitly not required?

---

_Comment by @MichaReiser on 2023-07-11 05:31_

Uhm what. No approve button on github mobile 🧐 *Approve*

---

_Comment by @dhruvmanila on 2023-07-11 17:16_

> Uhm what. No approve button on github mobile 🧐 _Approve_

It's hidden inside the file section > "Review Changes" button ;)

---

_@MichaReiser reviewed on 2023-07-17 09:55_

Damn, I never hit Submit on these comments

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/generated.rs`:38 on 2023-07-17 09:55_

I'm gonna revert the generated changes. See https://github.com/astral-sh/ruff/pull/5804 for why

---

_@MichaReiser reviewed on 2023-07-17 09:55_

---
