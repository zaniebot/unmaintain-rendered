```yaml
number: 4770
title: "[`flake8-pyi`] Implement PYI053"
type: pull_request
state: merged
author: density
labels:
  - rule
assignees: []
merged: true
base: main
head: PYI053
created_at: 2023-05-31T22:37:40Z
updated_at: 2023-06-01T23:18:00Z
url: https://github.com/astral-sh/ruff/pull/4770
synced_at: 2026-01-12T03:50:03Z
```

# [`flake8-pyi`] Implement PYI053

---

_Pull request opened by @density on 2023-05-31 22:37_

## Summary

Implement Y053 from https://github.com/PyCQA/flake8-pyi: `Only string and bytes literals <=50 characters long are permitted.`

## Test Plan

Added snapshot tests

ref: #848

---

_@density reviewed on 2023-05-31 22:38_

---

_Review comment by @density on `crates/ruff/src/rules/flake8_pyi/rules/long_string_or_bytes_in_stub.rs`:16 on 2023-05-31 22:38_

Questionable reasoning, but my archaeology in `flake8-pyi` didn't turn up any better reasons.

---

_Marked ready for review by @density on 2023-05-31 22:39_

---

_Renamed from "Implement PYI053" to "[`flake8-pyi`] Implement PYI053" by @density on 2023-05-31 22:40_

---

_@density reviewed on 2023-05-31 22:50_

---

_Review comment by @density on `crates/ruff/src/rules/flake8_pyi/rules/long_string_or_bytes_in_stub.rs`:1 on 2023-05-31 22:50_

`_in_stub` feels redundant in the name given that it's in `flake8_pyi`. lmk if you prefer a different name.

---

_Review requested from @charliermarsh by @charliermarsh on 2023-06-01 02:11_

---

_@charliermarsh reviewed on 2023-06-01 04:08_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/long_string_or_bytes_in_stub.rs`:16 on 2023-06-01 04:08_

Here's an explanation: https://github.com/PyCQA/flake8-pyi/blob/ceab86d16b822d12abae1d8087850d0bc66d2f52/CHANGELOG.md?plain=1#LL110C1-L115C23

---

_Review comment by @density on `crates/ruff/src/rules/flake8_pyi/rules/long_string_or_bytes_in_stub.rs`:16 on 2023-06-01 04:10_

Funny, good catch.

---

_@density reviewed on 2023-06-01 04:10_

---

_@charliermarsh reviewed on 2023-06-01 04:12_

Do you mind taking a look at the `is_valid_default_value_with_annotation` logic in `rules/flake8_pyi/rules/simple_defaults.rs`? It looks like these checks _used_ to be part of Y015, but were made more general and moved to their own rule (Y053) in flake8-pyi -- you can see the commit [here](https://github.com/PyCQA/flake8-pyi/commit/f6118974eed9a2feea170d7634c32b1eaa138316).

So when adding this, we should probably change the `is_valid_default_value_with_annotation` definition to ignore overlong strings (since they'll now be picked up by this new rule).

---

_Review requested from @charliermarsh by @charliermarsh on 2023-06-01 04:43_

---

_Comment by @density on 2023-06-01 04:53_

@charliermarsh Thanks for the feedback, updated.

---

_@MichaReiser approved on 2023-06-01 06:16_

---

_@density reviewed on 2023-06-01 13:15_

---

_Review comment by @density on `crates/ruff/src/rules/flake8_pyi/rules/long_string_or_bytes_in_stub.rs`:16 on 2023-06-01 13:15_

@charliermarsh I improved the comment and added an example.

---

_Label `rule` added by @charliermarsh on 2023-06-01 22:49_

---

_Merged by @charliermarsh on 2023-06-01 23:00_

---

_Closed by @charliermarsh on 2023-06-01 23:00_

---

_Comment by @github-actions[bot] on 2023-06-01 23:17_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.1±0.10ms     2.9 MB/sec    1.02     14.3±0.17ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    427.7±2.64µs     6.9 MB/sec    1.00    425.9±1.02µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.02ms     4.4 MB/sec    1.00      5.9±0.02ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.02ms     5.9 MB/sec    1.00      6.9±0.02ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1512.8±2.15µs    11.0 MB/sec    1.00   1516.6±4.10µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.9±0.59µs    17.3 MB/sec    1.00    171.7±0.72µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.2±0.01ms     7.9 MB/sec    1.00      5.2±0.00ms     7.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1013.3±8.19µs    16.4 MB/sec    1.00   1012.1±0.78µs    16.5 MB/sec
parser/numpy/globals.py                    1.00    104.7±0.16µs    28.2 MB/sec    1.00    105.0±0.41µs    28.1 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.5 MB/sec    1.00      2.2±0.00ms    11.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.06     17.9±0.21ms     2.3 MB/sec    1.00     16.9±0.14ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.04      4.5±0.06ms     3.7 MB/sec    1.00      4.4±0.05ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.02   462.4±10.96µs     6.4 MB/sec    1.00    454.4±7.47µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.05      7.6±0.09ms     3.4 MB/sec    1.00      7.3±0.09ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.10      9.4±0.09ms     4.3 MB/sec    1.00      8.5±0.06ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.07  1941.3±22.65µs     8.6 MB/sec    1.00  1811.2±28.41µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.05    207.8±3.06µs    14.2 MB/sec    1.00    198.6±5.22µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.08      4.2±0.03ms     6.1 MB/sec    1.00      3.9±0.02ms     6.6 MB/sec
parser/large/dataset.py                    1.00      6.6±0.04ms     6.1 MB/sec    1.22      8.1±0.07ms     5.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1261.5±16.53µs    13.2 MB/sec    1.17  1481.9±21.30µs    11.2 MB/sec
parser/numpy/globals.py                    1.00    131.6±1.81µs    22.4 MB/sec    1.12    147.4±1.28µs    20.0 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.0 MB/sec    1.18      3.3±0.02ms     7.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
