```yaml
number: 3616
title: "Use language: system for Rust hooks"
type: pull_request
state: merged
author: JonathanPlasse
labels:
  - internal
assignees: []
merged: true
base: main
head: pre-commit/use-rust-language-system
created_at: 2023-03-19T21:04:14Z
updated_at: 2023-03-21T07:23:38Z
url: https://github.com/astral-sh/ruff/pull/3616
synced_at: 2026-01-12T04:39:45Z
```

# Use language: system for Rust hooks

---

_Pull request opened by @JonathanPlasse on 2023-03-19 21:04_

Avoid cache conflicts (i.e. having to recompile everything after running pre-commit)

---

_Comment by @github-actions[bot] on 2023-03-19 21:14_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.04     15.6±0.44ms     2.6 MB/sec    1.00     15.0±0.32ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.18ms     4.1 MB/sec    1.00      4.1±0.15ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01   557.4±41.06µs     5.3 MB/sec    1.00   549.6±22.92µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.9±0.20ms     3.7 MB/sec    1.00      6.8±0.19ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.04      8.3±0.20ms     4.9 MB/sec    1.00      8.0±0.20ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1843.8±59.73µs     9.0 MB/sec    1.00  1805.2±70.84µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.02   224.6±23.05µs    13.1 MB/sec    1.00   221.0±11.06µs    13.3 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.9±0.12ms     6.6 MB/sec    1.00      3.8±0.14ms     6.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.8±0.32ms     2.9 MB/sec    1.02     14.0±1.14ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.12ms     4.8 MB/sec    1.00      3.5±0.15ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    407.6±3.05µs     7.2 MB/sec    1.00    408.5±1.76µs     7.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.42ms     4.3 MB/sec    1.03      6.1±0.70ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.6±0.10ms     5.4 MB/sec    1.01      7.6±0.12ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1573.1±69.19µs    10.6 MB/sec    1.00  1577.3±38.72µs    10.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   171.4±16.58µs    17.2 MB/sec    1.00    171.7±2.62µs    17.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.07ms     7.4 MB/sec    1.00      3.4±0.11ms     7.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-03-19 22:01_

What does this change exactly? Beyond that we avoid cache conflicts -- what does `language: system` vs. `language: rust` "mean" in pre-commit, if you know?

---

_Comment by @JonathanPlasse on 2023-03-19 22:10_

https://pre-commit.com/#rust
https://pre-commit.com/#system
> System hooks provide a way to write hooks for system-level executables which don't have a supported language above (or have special environment requirements that don't allow them to run in isolation such as pylint).
>
> This hook type will not be given a virtual environment to work with – if it needs additional dependencies the consumer must install them manually.

It will run the cargo commands as if it was directly on the machine and it will not create a virtual environment.

I think the problem I have is that rust on pre-commit and locally could use a different version which would conflict.
I should try to reproduce on docker and open an issue upstream to ask for more information.

---

_Comment by @andersk on 2023-03-19 22:16_

It looks like `language: rust` is designed for hermetically compiling and running a checker that’s written in Rust, not for checking your Rust code. That’s why it requires a `Cargo.toml` that produces at least one binary.

---

_Comment by @charliermarsh on 2023-03-20 23:35_

Ah ok, so this seems _correct_ then?

---

_Label `internal` added by @charliermarsh on 2023-03-20 23:37_

---

_Label `isort` added by @charliermarsh on 2023-03-20 23:37_

---

_Label `isort` removed by @charliermarsh on 2023-03-20 23:37_

---

_Merged by @charliermarsh on 2023-03-21 02:44_

---

_Closed by @charliermarsh on 2023-03-21 02:44_

---

_Branch deleted on 2023-03-21 07:23_

---
