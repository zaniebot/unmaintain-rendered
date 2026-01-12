```yaml
number: 4583
title: Add autofix for PYI009
type: pull_request
state: merged
author: qdegraaf
labels: []
assignees: []
merged: true
base: main
head: feature/autfoxiPYI009
created_at: 2023-05-22T15:46:53Z
updated_at: 2023-05-23T08:12:41Z
url: https://github.com/astral-sh/ruff/pull/4583
synced_at: 2026-01-12T15:55:15Z
```

# Add autofix for PYI009

---

_@qdegraaf_

Checked a recent autofix addition PR (https://github.com/charliermarsh/ruff/pull/4178/files#diff-6abdc1e4d11a89870a0a863b5132d8fa2f793889f56d1a1fa95fb0a0ce831486) for what I needed to change. 

If I understand `checker.patch` and autofix correctly, this should cover it. Happy to hear if I've missed anything.

Closes: https://github.com/charliermarsh/ruff/issues/4580 



---

_Review comment by @Skylion007 on `crates/ruff/src/rules/flake8_pyi/rules/pass_statement_stub_body.rs`:19 on 2023-05-22 15:51_

Could be empty class or function body, right?

---

_@Skylion007 reviewed on 2023-05-22 15:51_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/pass_statement_stub_body.rs`:19 on 2023-05-22 15:52_

I think "Replace with `...`" is sufficient here

---

_@charliermarsh reviewed on 2023-05-22 15:52_

---

_Comment by @charliermarsh on 2023-05-22 15:53_

Do you mind tweaking the suggestion and re-generating the test snapshots?

---

_Comment by @charliermarsh on 2023-05-22 15:55_

(`cargo test`, then `cargo insta review`)

---

_Comment by @github-actions[bot] on 2023-05-22 16:21_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.1±0.10ms     2.7 MB/sec    1.01     15.2±0.07ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.6 MB/sec    1.01      3.7±0.01ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    363.9±1.01µs     8.1 MB/sec    1.02    369.6±1.43µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.01      6.3±0.02ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.01ms     5.5 MB/sec    1.01      7.5±0.01ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1531.7±3.48µs    10.9 MB/sec    1.02   1556.6±7.30µs    10.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    163.7±2.33µs    18.0 MB/sec    1.02    167.3±4.05µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.8 MB/sec    1.01      3.3±0.01ms     7.7 MB/sec
parser/large/dataset.py                    1.00      5.8±0.00ms     7.0 MB/sec    1.04      6.0±0.01ms     6.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1133.0±1.23µs    14.7 MB/sec    1.04   1175.8±2.70µs    14.2 MB/sec
parser/numpy/globals.py                    1.00    117.4±0.26µs    25.1 MB/sec    1.04    121.8±0.57µs    24.2 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.3 MB/sec    1.03      2.6±0.02ms    10.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     21.9±0.77ms  1904.1 KB/sec    1.03     22.5±0.78ms  1851.5 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.6±0.26ms     3.0 MB/sec    1.03      5.7±0.36ms     2.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   647.4±33.31µs     4.6 MB/sec    1.03   668.8±81.55µs     4.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.2±0.34ms     2.8 MB/sec    1.05      9.7±0.42ms     2.6 MB/sec
linter/default-rules/large/dataset.py      1.00     10.8±0.39ms     3.8 MB/sec    1.03     11.2±0.35ms     3.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.3±0.12ms     7.3 MB/sec    1.03      2.3±0.10ms     7.1 MB/sec
linter/default-rules/numpy/globals.py      1.00   266.2±14.44µs    11.1 MB/sec    1.04   277.8±19.33µs    10.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.9±0.18ms     5.2 MB/sec    1.02      5.0±0.21ms     5.1 MB/sec
parser/large/dataset.py                    1.00      8.9±0.34ms     4.6 MB/sec    1.04      9.2±0.31ms     4.4 MB/sec
parser/numpy/ctypeslib.py                  1.00  1715.6±82.83µs     9.7 MB/sec    1.03  1767.5±55.10µs     9.4 MB/sec
parser/numpy/globals.py                    1.00   173.7±11.57µs    17.0 MB/sec    1.05   182.4±11.21µs    16.2 MB/sec
parser/pydantic/types.py                   1.00      3.8±0.12ms     6.8 MB/sec    1.03      3.9±0.16ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @qdegraaf on 2023-05-22 16:31_

> Do you mind tweaking the suggestion and re-generating the test snapshots?

I don't mind. Done!

---

_Merged by @charliermarsh on 2023-05-22 16:41_

---

_Closed by @charliermarsh on 2023-05-22 16:41_

---

_Branch deleted on 2023-05-23 08:12_

---
