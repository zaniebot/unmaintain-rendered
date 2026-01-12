```yaml
number: 3728
title: "[`flake8-pyi`] Implement `PYI015`"
type: pull_request
state: merged
author: JonathanPlasse
labels:
  - rule
assignees: []
merged: true
base: main
head: add-pyi015
created_at: 2023-03-25T11:52:57Z
updated_at: 2023-03-25T15:55:15Z
url: https://github.com/astral-sh/ruff/pull/3728
synced_at: 2026-01-12T04:39:45Z
```

# [`flake8-pyi`] Implement `PYI015`

---

_Pull request opened by @JonathanPlasse on 2023-03-25 11:52_

```console
❯ cargo run -p ruff_cli -- check --no-cache crates/ruff/resources/test/fixtures/flake8_pyi/PYI015.pyi --select=PYI015
   Compiling ruff v0.0.259 (/home/jonathan/Documents/github/ruff/crates/ruff)
   Compiling ruff_cli v0.0.259 (/home/jonathan/Documents/github/ruff/crates/ruff_cli)
    Finished dev [unoptimized + debuginfo] target(s) in 11.80s
     Running `target/debug/ruff check --no-cache crates/ruff/resources/test/fixtures/flake8_pyi/PYI015.pyi --select=PYI015`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI015.pyi:40:23: PYI015 [*] Only simple default values allowed for assignments
crates/ruff/resources/test/fixtures/flake8_pyi/PYI015.pyi:41:23: PYI015 [*] Only simple default values allowed for assignments
crates/ruff/resources/test/fixtures/flake8_pyi/PYI015.pyi:42:23: PYI015 [*] Only simple default values allowed for assignments
crates/ruff/resources/test/fixtures/flake8_pyi/PYI015.pyi:43:26: PYI015 [*] Only simple default values allowed for assignments
crates/ruff/resources/test/fixtures/flake8_pyi/PYI015.pyi:44:47: PYI015 [*] Only simple default values allowed for assignments
crates/ruff/resources/test/fixtures/flake8_pyi/PYI015.pyi:45:31: PYI015 [*] Only simple default values allowed for assignments
crates/ruff/resources/test/fixtures/flake8_pyi/PYI015.pyi:46:37: PYI015 [*] Only simple default values allowed for assignments
crates/ruff/resources/test/fixtures/flake8_pyi/PYI015.pyi:48:28: PYI015 [*] Only simple default values allowed for assignments
crates/ruff/resources/test/fixtures/flake8_pyi/PYI015.pyi:49:11: PYI015 [*] Only simple default values allowed for assignments
crates/ruff/resources/test/fixtures/flake8_pyi/PYI015.pyi:50:11: PYI015 [*] Only simple default values allowed for assignments
crates/ruff/resources/test/fixtures/flake8_pyi/PYI015.pyi:51:11: PYI015 [*] Only simple default values allowed for assignments
Found 11 errors.
[*] 11 potentially fixable with the --fix option.
❯ flake8 crates/ruff/resources/test/fixtures/flake8_pyi/PYI015.pyi --select=Y015 | wc -l
11
```

---

_Comment by @github-actions[bot] on 2023-03-25 12:24_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     14.1±0.06ms     2.9 MB/sec    1.00     13.7±0.09ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.6 MB/sec    1.00      3.6±0.00ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    495.6±1.00µs     6.0 MB/sec    1.00    496.8±1.10µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.1±0.04ms     4.2 MB/sec    1.00      6.0±0.02ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.03      7.4±0.03ms     5.5 MB/sec    1.00      7.2±0.01ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1665.1±1.66µs    10.0 MB/sec    1.00   1643.9±6.53µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    186.0±1.43µs    15.9 MB/sec    1.00    184.3±1.24µs    16.0 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.5±0.01ms     7.3 MB/sec    1.00      3.4±0.02ms     7.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     18.3±0.48ms     2.2 MB/sec    1.01     18.4±0.50ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.0±0.19ms     3.4 MB/sec    1.00      5.0±0.17ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   634.4±21.04µs     4.7 MB/sec    1.00   632.4±20.89µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.1±0.26ms     3.1 MB/sec    1.00      8.1±0.26ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.00      9.8±0.24ms     4.1 MB/sec    1.03     10.1±0.37ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.2±0.07ms     7.6 MB/sec    1.00      2.2±0.10ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   242.1±13.52µs    12.2 MB/sec    1.01   243.7±15.33µs    12.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.14ms     5.6 MB/sec    1.02      4.7±0.21ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `rule` added by @charliermarsh on 2023-03-25 15:34_

---

_Renamed from "Add PYI015 rule" to "[`flake8-pyi`] Implement `PYI015`" by @charliermarsh on 2023-03-25 15:35_

---

_Comment by @charliermarsh on 2023-03-25 15:45_

Rock on thx

---

_Merged by @charliermarsh on 2023-03-25 15:48_

---

_Closed by @charliermarsh on 2023-03-25 15:48_

---

_Branch deleted on 2023-03-25 15:55_

---
