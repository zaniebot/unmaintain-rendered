```yaml
number: 4146
title: "Change `--fix-only` exit semantics to mirror `--fix`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - breaking
  - cli
assignees: []
merged: true
base: main
head: charlie/fix-only
created_at: 2023-04-29T04:08:31Z
updated_at: 2023-05-04T19:26:10Z
url: https://github.com/astral-sh/ruff/pull/4146
synced_at: 2026-01-12T15:55:14Z
```

# Change `--fix-only` exit semantics to mirror `--fix`

---

_@charliermarsh_

## Summary

After this change, running `ruff --fix-only` will exit with status code 0 unless the program hits an internal error, while `ruff --fix-only --exit-non-zero-on-fix` will exit with status code 1 if at least one diagnostic is fixed. This effectively mirrors `ruff --fix`, taking into account that non-fixable diagnostics are ignored. Previously, the `--exit-non-zero-on-fix` flag had no effect when combined with `--fix-only`.

Closes #4092.

---

_Label `breaking` added by @charliermarsh on 2023-04-29 04:08_

---

_Label `cli` added by @charliermarsh on 2023-04-29 04:08_

---

_Comment by @github-actions[bot] on 2023-04-29 04:18_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.8±0.53ms     2.7 MB/sec    1.01     15.0±0.51ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.18ms     4.7 MB/sec    1.02      3.6±0.09ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.06   459.8±14.85µs     6.4 MB/sec    1.00   432.0±21.92µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.37ms     4.3 MB/sec    1.02      6.1±0.25ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.49ms     5.7 MB/sec    1.04      7.4±0.20ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1614.2±50.54µs    10.3 MB/sec    1.00  1593.5±33.90µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    177.2±4.62µs    16.7 MB/sec    1.00    178.1±5.58µs    16.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.07ms     7.8 MB/sec    1.05      3.5±0.09ms     7.4 MB/sec
parser/large/dataset.py                    1.00      5.6±0.31ms     7.3 MB/sec    1.05      5.8±0.16ms     7.0 MB/sec
parser/numpy/ctypeslib.py                  1.00  1122.1±28.38µs    14.8 MB/sec    1.03  1160.6±40.73µs    14.3 MB/sec
parser/numpy/globals.py                    1.01    117.4±4.59µs    25.1 MB/sec    1.00    115.8±4.28µs    25.5 MB/sec
parser/pydantic/types.py                   1.00      2.4±0.05ms    10.4 MB/sec    1.02      2.5±0.06ms    10.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     12.4±0.17ms     3.3 MB/sec    1.00     12.1±0.08ms     3.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.2±0.03ms     5.3 MB/sec    1.00      3.1±0.02ms     5.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    321.0±4.98µs     9.2 MB/sec    1.02    328.2±6.32µs     9.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.3±0.05ms     4.9 MB/sec    1.00      5.3±0.08ms     4.8 MB/sec
linter/default-rules/large/dataset.py      1.02      6.3±0.05ms     6.5 MB/sec    1.00      6.2±0.04ms     6.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1329.1±12.29µs    12.5 MB/sec    1.00  1316.6±12.05µs    12.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    138.9±1.78µs    21.2 MB/sec    1.02    141.6±2.50µs    20.8 MB/sec
linter/default-rules/pydantic/types.py     1.02      2.9±0.03ms     8.9 MB/sec    1.00      2.8±0.02ms     9.1 MB/sec
parser/large/dataset.py                    1.00      4.9±0.05ms     8.3 MB/sec    1.01      4.9±0.05ms     8.3 MB/sec
parser/numpy/ctypeslib.py                  1.00    926.4±7.05µs    18.0 MB/sec    1.00    928.9±8.82µs    17.9 MB/sec
parser/numpy/globals.py                    1.00     95.4±1.06µs    30.9 MB/sec    1.00     95.8±1.38µs    30.8 MB/sec
parser/pydantic/types.py                   1.00      2.1±0.02ms    12.3 MB/sec    1.00      2.1±0.02ms    12.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/lib.rs`:296 on 2023-04-29 15:54_

I'm a bit confused. Isn't `diff` `--fix`  and `fix_only` `--fix-only`? If so, shouldn't the `cli.diff` and `fix_only` branches be identical?

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/lib.rs`:295 on 2023-04-29 15:55_

Nit: Safe to fallthrough.
```suggestion
                }
```

---

_@MichaReiser reviewed on 2023-04-29 15:56_

---

_@charliermarsh reviewed on 2023-05-02 06:11_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/lib.rs`:296 on 2023-05-02 06:11_

No, those aren't the same. `--fix`, `--fix-only`, and `--diff` are all different options.

---

_@MichaReiser approved on 2023-05-02 07:17_

---

_Comment by @MichaReiser on 2023-05-02 07:18_

Nit: It may be helpful to add some documentation explaining why/when we exit with a negative exit code for each option. 

---

_Merged by @charliermarsh on 2023-05-04 19:03_

---

_Closed by @charliermarsh on 2023-05-04 19:03_

---

_Branch deleted on 2023-05-04 19:03_

---
