```yaml
number: 4328
title: "Impl `Default` for `SourceLocation`"
type: pull_request
state: merged
author: youknowone
labels:
  - internal
assignees: []
merged: true
base: main
head: source-location
created_at: 2023-05-09T20:23:30Z
updated_at: 2023-05-16T07:22:49Z
url: https://github.com/astral-sh/ruff/pull/4328
synced_at: 2026-01-12T15:55:15Z
```

# Impl `Default` for `SourceLocation`

---

_@youknowone_

`MIN` is useful for fallback values.

`Copy` will be cheap once #4288 is done

---

_Comment by @github-actions[bot] on 2023-05-09 21:12_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     21.4±0.50ms  1948.3 KB/sec    1.00     21.3±0.63ms  1958.1 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.32ms     3.2 MB/sec    1.01      5.3±0.24ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.02   587.9±30.07µs     5.0 MB/sec    1.00   578.3±23.29µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.8±0.27ms     2.9 MB/sec    1.02      9.0±0.38ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00      9.9±0.44ms     4.1 MB/sec    1.04     10.3±0.50ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1976.8±80.30µs     8.4 MB/sec    1.00  1968.4±85.29µs     8.5 MB/sec
linter/default-rules/numpy/globals.py      1.00   227.6±13.29µs    13.0 MB/sec    1.01   231.0±13.68µs    12.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.19ms     6.0 MB/sec    1.07      4.5±0.20ms     5.6 MB/sec
parser/large/dataset.py                    1.00      7.6±0.17ms     5.3 MB/sec    1.01      7.7±0.31ms     5.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1419.3±58.01µs    11.7 MB/sec    1.03  1459.2±88.46µs    11.4 MB/sec
parser/numpy/globals.py                    1.00    140.6±6.00µs    21.0 MB/sec    1.00    141.2±5.17µs    20.9 MB/sec
parser/pydantic/types.py                   1.00      3.2±0.10ms     8.1 MB/sec    1.02      3.2±0.16ms     7.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     16.6±0.17ms     2.5 MB/sec    1.00     16.4±0.10ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.03ms     3.9 MB/sec    1.00      4.2±0.05ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    504.8±6.86µs     5.8 MB/sec    1.00   502.6±11.06µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.07ms     3.7 MB/sec    1.00      6.9±0.05ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.05ms     5.0 MB/sec    1.02      8.2±0.05ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1726.9±22.01µs     9.6 MB/sec    1.01  1741.9±15.56µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    195.7±5.34µs    15.1 MB/sec    1.03    200.6±7.14µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.0 MB/sec    1.03      3.8±0.17ms     6.8 MB/sec
parser/large/dataset.py                    1.01      6.7±0.06ms     6.1 MB/sec    1.00      6.6±0.04ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.01  1270.2±18.06µs    13.1 MB/sec    1.00  1254.6±11.62µs    13.3 MB/sec
parser/numpy/globals.py                    1.00    130.0±1.71µs    22.7 MB/sec    1.00    129.3±2.42µs    22.8 MB/sec
parser/pydantic/types.py                   1.01      2.8±0.03ms     9.0 MB/sec    1.00      2.8±0.03ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/message/azure.rs`:22 on 2023-05-10 05:20_

I think we could use ::default here instead of min. That would also allow to use unwrap_or_default in many places

---

_@MichaReiser reviewed on 2023-05-10 05:20_

---

_Renamed from "SourceLocation::MIN and copy" to "Impl `Default` for `SourceLocation`" by @MichaReiser on 2023-05-16 06:53_

---

_Label `internal` added by @MichaReiser on 2023-05-16 06:53_

---

_Merged by @MichaReiser on 2023-05-16 07:03_

---

_Closed by @MichaReiser on 2023-05-16 07:03_

---
