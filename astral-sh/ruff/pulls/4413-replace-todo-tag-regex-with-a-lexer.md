```yaml
number: 4413
title: Replace TODO tag regex with a lexer
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/enum
created_at: 2023-05-13T15:07:40Z
updated_at: 2023-05-14T01:59:32Z
url: https://github.com/astral-sh/ruff/pull/4413
synced_at: 2026-01-12T15:55:15Z
```

# Replace TODO tag regex with a lexer

---

_@charliermarsh_

A little faster in my scrappy hyperfine benchmark.

---

_Merged by @charliermarsh on 2023-05-13 15:23_

---

_Closed by @charliermarsh on 2023-05-13 15:23_

---

_Branch deleted on 2023-05-13 15:23_

---

_Comment by @github-actions[bot] on 2023-05-13 15:24_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.9±0.13ms     2.7 MB/sec    1.03     15.4±0.16ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.02ms     4.6 MB/sec    1.03      3.7±0.02ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    364.2±1.13µs     8.1 MB/sec    1.01    367.3±0.67µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.03ms     4.1 MB/sec    1.04      6.4±0.07ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.05ms     5.6 MB/sec    1.08      7.9±0.04ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1511.7±3.69µs    11.0 MB/sec    1.06   1605.1±6.24µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    162.9±0.86µs    18.1 MB/sec    1.05    170.9±0.31µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.8 MB/sec    1.07      3.5±0.01ms     7.3 MB/sec
parser/large/dataset.py                    1.00      5.8±0.01ms     7.0 MB/sec    1.06      6.2±0.01ms     6.6 MB/sec
parser/numpy/ctypeslib.py                  1.00   1136.8±1.23µs    14.6 MB/sec    1.05   1194.5±2.92µs    13.9 MB/sec
parser/numpy/globals.py                    1.00    116.6±0.28µs    25.3 MB/sec    1.04    121.7±0.84µs    24.2 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.00ms    10.3 MB/sec    1.05      2.6±0.01ms     9.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.04     19.9±0.86ms     2.0 MB/sec    1.00     19.1±0.54ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      5.0±0.16ms     3.3 MB/sec    1.00      4.9±0.15ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   584.5±18.62µs     5.0 MB/sec    1.00   585.1±23.73µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.04      8.3±0.30ms     3.1 MB/sec    1.00      8.0±0.29ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      9.3±0.30ms     4.4 MB/sec    1.04      9.7±0.37ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03      2.1±0.15ms     8.0 MB/sec    1.00      2.0±0.07ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    235.0±9.43µs    12.6 MB/sec    1.02   239.7±12.30µs    12.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.20ms     6.0 MB/sec    1.00      4.3±0.17ms     6.0 MB/sec
parser/large/dataset.py                    1.00      8.0±0.21ms     5.1 MB/sec    1.01      8.1±0.37ms     5.0 MB/sec
parser/numpy/ctypeslib.py                  1.03  1543.8±58.41µs    10.8 MB/sec    1.00  1495.6±62.66µs    11.1 MB/sec
parser/numpy/globals.py                    1.04    164.8±7.62µs    17.9 MB/sec    1.00    158.5±7.33µs    18.6 MB/sec
parser/pydantic/types.py                   1.00      3.5±0.09ms     7.3 MB/sec    1.00      3.5±0.10ms     7.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todos/rules.rs`:236 on 2023-05-13 19:51_

Use `TextSize` for offsets to make it clear, that this is a text offset (and e.g. not an offset into an array)

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todos/rules.rs`:254 on 2023-05-13 19:54_

Nit: I wonder if it would be easier to use string operations instead

```
let trimmed = comment.trim_start_matches('#').trim_start();
let offset = comment.text_len() - trimmed.text_len();

// rest
...
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todos/rules.rs`:282 on 2023-05-13 19:54_

Nit: Return a `TextSize` instead to make it clear, that this is a text offset

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todos/rules.rs`:353 on 2023-05-13 19:54_

```
let (offset, directive) = Directive::from_comment(comment)?;
```

---

_@MichaReiser reviewed on 2023-05-13 19:55_

---

_@charliermarsh reviewed on 2023-05-14 01:59_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_todos/rules.rs`:236 on 2023-05-14 01:59_

https://github.com/charliermarsh/ruff/pull/4426

---

_@charliermarsh reviewed on 2023-05-14 01:59_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_todos/rules.rs`:254 on 2023-05-14 01:59_

https://github.com/charliermarsh/ruff/pull/4426

---

_@charliermarsh reviewed on 2023-05-14 01:59_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_todos/rules.rs`:282 on 2023-05-14 01:59_

https://github.com/charliermarsh/ruff/pull/4426

---

_@charliermarsh reviewed on 2023-05-14 01:59_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_todos/rules.rs`:353 on 2023-05-14 01:59_

https://github.com/charliermarsh/ruff/pull/4426

---
