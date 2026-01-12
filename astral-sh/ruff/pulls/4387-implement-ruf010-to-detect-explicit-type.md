```yaml
number: 4387
title: "Implement `RUF010` to detect explicit type conversions within f-strings"
type: pull_request
state: merged
author: LotemAm
labels:
  - rule
assignees: []
merged: true
base: main
head: f-string-conversion
created_at: 2023-05-11T23:52:22Z
updated_at: 2023-05-12T18:32:22Z
url: https://github.com/astral-sh/ruff/pull/4387
synced_at: 2026-01-12T03:50:02Z
```

# Implement `RUF010` to detect explicit type conversions within f-strings

---

_Pull request opened by @LotemAm on 2023-05-11 23:52_

This rule will check if an f-string contains anything that can be replace with a conversion (e.g. `str(foo)` to `foo!s`)

Sorry if there are any glaring Rust mistakes, this is my first time with the language

---

_Comment by @github-actions[bot] on 2023-05-12 00:13_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.6±0.11ms     2.8 MB/sec    1.00     14.4±0.05ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.01    364.1±1.78µs     8.1 MB/sec    1.00    361.3±1.48µs     8.2 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.1±0.01ms     4.2 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.3±0.02ms     5.6 MB/sec    1.00      7.2±0.02ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1530.4±3.38µs    10.9 MB/sec    1.00   1507.0±3.78µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    165.2±0.34µs    17.9 MB/sec    1.00    164.5±1.16µs    17.9 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.3±0.02ms     7.8 MB/sec    1.00      3.2±0.01ms     7.9 MB/sec
parser/large/dataset.py                    1.00      5.8±0.01ms     7.1 MB/sec    1.00      5.8±0.01ms     7.0 MB/sec
parser/numpy/ctypeslib.py                  1.00   1120.7±2.59µs    14.9 MB/sec    1.02  1137.6±66.70µs    14.6 MB/sec
parser/numpy/globals.py                    1.00    114.7±0.34µs    25.7 MB/sec    1.00    114.6±0.17µs    25.7 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.4 MB/sec    1.00      2.5±0.01ms    10.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.2±0.15ms     2.5 MB/sec    1.00     16.3±0.30ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.04ms     4.0 MB/sec    1.00      4.1±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    486.7±6.26µs     6.1 MB/sec    1.00    485.4±5.08µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.06ms     3.7 MB/sec    1.00      6.8±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.07ms     5.0 MB/sec    1.00      8.1±0.05ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1718.9±40.00µs     9.7 MB/sec    1.01  1728.8±17.29µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    194.3±3.85µs    15.2 MB/sec    1.00    195.0±3.69µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.03ms     6.9 MB/sec    1.00      3.7±0.04ms     7.0 MB/sec
parser/large/dataset.py                    1.00      6.6±0.06ms     6.1 MB/sec    1.00      6.6±0.04ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.00  1245.3±11.95µs    13.4 MB/sec    1.01  1255.7±14.26µs    13.3 MB/sec
parser/numpy/globals.py                    1.00    128.3±3.29µs    23.0 MB/sec    1.00    128.2±3.01µs    23.0 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.1 MB/sec    1.00      2.8±0.03ms     9.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/f_string_conversion.rs`:55 on 2023-05-12 07:27_

I think we must check that the functions resolve to the `builtin` functions and not to user-defined functions. 

---

_@MichaReiser approved on 2023-05-12 07:28_

LGTM. Thank you

---

_@LotemAm reviewed on 2023-05-12 09:13_

---

_Review comment by @LotemAm on `crates/ruff/src/rules/ruff/rules/f_string_conversion.rs`:55 on 2023-05-12 09:13_

Agreed, I added a check

---

_@MichaReiser reviewed on 2023-05-12 09:26_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/f_string_conversion.rs`:55 on 2023-05-12 09:26_

I think you can do `checker.ctx.is_builtin(id)` directly

---

_@LotemAm reviewed on 2023-05-12 09:33_

---

_Review comment by @LotemAm on `crates/ruff/src/rules/ruff/rules/f_string_conversion.rs`:55 on 2023-05-12 09:33_

Much better, thanks!

---

_Label `rule` added by @charliermarsh on 2023-05-12 12:49_

---

_Comment by @charliermarsh on 2023-05-12 16:56_

Will merge this after cutting the release.

---

_Renamed from "Added RUF010 rule - use f-string conversions" to "Implement `RUF010` to detect explicit type conversions within f-strings" by @charliermarsh on 2023-05-12 18:08_

---

_Merged by @charliermarsh on 2023-05-12 18:12_

---

_Closed by @charliermarsh on 2023-05-12 18:12_

---
