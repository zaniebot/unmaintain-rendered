```yaml
number: 4215
title: End of statement insertion should occur after newline
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: main
head: add-import-after-newline
created_at: 2023-05-04T01:10:08Z
updated_at: 2023-05-04T14:23:11Z
url: https://github.com/astral-sh/ruff/pull/4215
synced_at: 2026-01-12T04:28:19Z
```

# End of statement insertion should occur after newline

---

_Pull request opened by @dhruvmanila on 2023-05-04 01:10_

### To-Do:

- [x] Update documentation for `end_of_statement_insertion`. Currently, it's same as that of `top_of_file_insertion` ;)
- [ ] ~Add test cases for `end_of_statement_insertion` if possible (refer to `end_of_statement_insertion` test cases)~ This seems to be a bit difficult due to the dependency on `Checker`
- [x] Add test case for `SIM105` with an existing non-contextlib import (testing `end_of_statement_insertion`)
- [x] Add test case for `SIM105` with `contextlib` already imported

fixes: #4203

---

_Comment by @github-actions[bot] on 2023-05-04 01:20_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     14.8±0.11ms     2.8 MB/sec    1.00     14.3±0.10ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.03ms     4.8 MB/sec    1.00      3.4±0.03ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    421.9±0.92µs     7.0 MB/sec    1.00    416.7±0.82µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.04      6.1±0.06ms     4.1 MB/sec    1.00      5.9±0.05ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.03      7.2±0.05ms     5.7 MB/sec    1.00      7.0±0.03ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1501.9±5.64µs    11.1 MB/sec    1.00   1493.3±3.71µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.01    168.4±0.25µs    17.5 MB/sec    1.00    166.5±0.30µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.2±0.02ms     8.1 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.4±0.01ms     7.5 MB/sec    1.00      5.5±0.01ms     7.4 MB/sec
parser/numpy/ctypeslib.py                  1.00   1060.0±4.61µs    15.7 MB/sec    1.00   1059.8±2.34µs    15.7 MB/sec
parser/numpy/globals.py                    1.00    107.8±0.31µs    27.4 MB/sec    1.00    108.3±0.43µs    27.2 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.0 MB/sec    1.00      2.3±0.00ms    11.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     20.7±0.35ms  2012.3 KB/sec    1.00     20.8±0.39ms  2004.3 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.14ms     3.3 MB/sec    1.00      5.1±0.11ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   602.4±31.41µs     4.9 MB/sec    1.00   599.7±21.69µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.7±0.20ms     2.9 MB/sec    1.00      8.7±0.21ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00     10.4±0.22ms     3.9 MB/sec    1.00     10.3±0.19ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.2±0.06ms     7.6 MB/sec    1.00      2.2±0.07ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    243.4±8.08µs    12.1 MB/sec    1.03   249.6±11.76µs    11.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.11ms     5.5 MB/sec    1.00      4.6±0.14ms     5.5 MB/sec
parser/large/dataset.py                    1.01      8.3±0.12ms     4.9 MB/sec    1.00      8.2±0.11ms     5.0 MB/sec
parser/numpy/ctypeslib.py                  1.02  1595.2±49.16µs    10.4 MB/sec    1.00  1563.3±45.80µs    10.7 MB/sec
parser/numpy/globals.py                    1.00    162.3±8.88µs    18.2 MB/sec    1.00    161.6±5.71µs    18.3 MB/sec
parser/pydantic/types.py                   1.01      3.5±0.07ms     7.3 MB/sec    1.00      3.5±0.08ms     7.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @dhruvmanila on 2023-05-04 13:52_

---

_Review comment by @MichaReiser on `crates/ruff/src/importer.rs`:189 on 2023-05-04 14:17_

whoops  :flushed:  ... my bad. Thanks for fixing this and adding tests.

---

_@MichaReiser approved on 2023-05-04 14:17_

---

_Merged by @MichaReiser on 2023-05-04 14:17_

---

_Closed by @MichaReiser on 2023-05-04 14:17_

---

_@dhruvmanila reviewed on 2023-05-04 14:21_

---

_Review comment by @dhruvmanila on `crates/ruff/src/importer.rs`:189 on 2023-05-04 14:21_

Happy to help! :)

---

_Branch deleted on 2023-05-04 14:23_

---
