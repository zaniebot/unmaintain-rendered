```yaml
number: 6677
title: "`ruff_formatter` crate doc comment fixes"
type: pull_request
state: merged
author: cnpryer
labels: []
assignees: []
merged: true
base: main
head: ruff-formatter-comments
created_at: 2023-08-18T15:36:49Z
updated_at: 2023-08-19T16:44:54Z
url: https://github.com/astral-sh/ruff/pull/6677
synced_at: 2026-01-12T15:55:22Z
```

# `ruff_formatter` crate doc comment fixes

---

_@cnpryer_

This PR updates some of the generated docs for the `ruff_formatter` crate. I figure I could throw some of these up as I'm reading through it. 

I'd like to leave this as a draft PR so that I can ask questions with review comments if that's okay.

---

_Comment by @github-actions[bot] on 2023-08-18 19:35_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.3±0.01ms    12.3 MB/sec    1.04      3.4±0.02ms    11.8 MB/sec
formatter/numpy/ctypeslib.py               1.00    692.5±2.12µs    24.0 MB/sec    1.04    718.7±3.37µs    23.2 MB/sec
formatter/numpy/globals.py                 1.00     72.9±0.34µs    40.5 MB/sec    1.06     77.1±0.30µs    38.3 MB/sec
formatter/pydantic/types.py                1.00   1333.9±7.81µs    19.1 MB/sec    1.06  1416.0±143.07µs    18.0 MB/sec
linter/all-rules/large/dataset.py          1.00     10.4±0.06ms     3.9 MB/sec    1.00     10.3±0.06ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      2.8±0.01ms     6.0 MB/sec    1.00      2.8±0.01ms     6.0 MB/sec
linter/all-rules/numpy/globals.py          1.01    393.6±2.42µs     7.5 MB/sec    1.00    389.0±1.13µs     7.6 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.4±0.05ms     4.7 MB/sec    1.00      5.4±0.03ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.00      5.5±0.02ms     7.4 MB/sec    1.00      5.5±0.02ms     7.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1210.6±19.41µs    13.8 MB/sec    1.00   1204.4±3.60µs    13.8 MB/sec
linter/default-rules/numpy/globals.py      1.06    147.7±6.39µs    20.0 MB/sec    1.00    139.7±0.39µs    21.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      2.5±0.06ms    10.3 MB/sec    1.00      2.4±0.01ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      3.7±0.13ms    11.0 MB/sec    1.00      3.7±0.08ms    11.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   726.9±12.30µs    22.9 MB/sec    1.01   733.5±13.36µs    22.7 MB/sec
formatter/numpy/globals.py                 1.00     75.5±2.11µs    39.1 MB/sec    1.02     77.2±2.37µs    38.2 MB/sec
formatter/pydantic/types.py                1.00  1508.1±40.62µs    16.9 MB/sec    1.00  1507.0±23.39µs    16.9 MB/sec
linter/all-rules/large/dataset.py          1.00     12.7±0.12ms     3.2 MB/sec    1.00     12.8±0.12ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.05ms     4.7 MB/sec    1.00      3.6±0.08ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    371.3±7.10µs     7.9 MB/sec    1.00    372.0±8.67µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.09ms     3.8 MB/sec    1.00      6.7±0.08ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.06ms     5.8 MB/sec    1.00      7.0±0.07ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1474.5±19.91µs    11.3 MB/sec    1.01  1482.5±16.34µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    150.4±2.15µs    19.6 MB/sec    1.02    153.3±7.82µs    19.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.2±0.08ms     8.0 MB/sec    1.00      3.2±0.03ms     8.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @cnpryer on `crates/ruff_formatter/src/macros.rs`:119 on 2023-08-18 20:23_

This is technically correct both ways. TIL

Edit: Tried using a suggestion to offer a fake revert lol. I'll just revert.

---

_@cnpryer reviewed on 2023-08-18 20:23_

---

_Comment by @cnpryer on 2023-08-18 23:55_

Hmm. Yea my questions so far are just specific to the `Printer` implementation. So I'll just throw those on discord and mark this as ready.

---

_Marked ready for review by @cnpryer on 2023-08-18 23:55_

---

_Renamed from "`ruff_formatter` docs improvements" to "`ruff_formatter` docs and comments improvements" by @cnpryer on 2023-08-18 23:55_

---

_@cnpryer reviewed on 2023-08-19 00:04_

---

_Review comment by @cnpryer on `crates/ruff_formatter/src/macros.rs`:119 on 2023-08-19 00:04_

933e8d11967dc9ff5bca9af106f9890edbdd3cbc

---

_Renamed from "`ruff_formatter` docs and comments improvements" to "`ruff_formatter` crate doc comment fixes" by @cnpryer on 2023-08-19 00:14_

---

_Comment by @MichaReiser on 2023-08-19 16:41_

Thank you

---

_Merged by @MichaReiser on 2023-08-19 16:42_

---

_Closed by @MichaReiser on 2023-08-19 16:42_

---

_Branch deleted on 2023-08-19 16:44_

---
