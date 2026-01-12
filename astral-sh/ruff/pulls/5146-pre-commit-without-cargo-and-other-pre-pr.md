```yaml
number: 5146
title: Pre commit without cargo and other pre-PR improvements
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: pre-commit_with_cargo
created_at: 2023-06-16T12:39:45Z
updated_at: 2023-06-18T11:24:18Z
url: https://github.com/astral-sh/ruff/pull/5146
synced_at: 2026-01-12T15:55:17Z
```

# Pre commit without cargo and other pre-PR improvements

---

_@konstin_

This tackles three problems:
* pre-commit was slow because it ran cargo commands
* Improve the clarity on what you need to run to get your PR pass on CI (and make those fast)
* You had to compile and run `cargo dev generate-all` separately, which was slow

The first change is to remove all cargo commands except running ruff itself from pre-commit. With `cargo run --bin ruff` already compiled it takes about 7s on my machine. It would make sense to also use the ruff pre-commit action here even if we're then lagging a release behind for checking ruff on ruff.

The contributing guide is now clear about what you need to run:

```shell
cargo clippy --workspace --all-targets --all-features -- -D warnings  # Linting...
RUFF_UPDATE_SCHEMA=1 cargo test  # Testing and updating ruff.schema.json
pre-commit run --all-files  # rust and python formatting, markdown and python linting, etc.
```

Example timings from my machine:

`cargo clippy --workspace --all-targets --all-features -- -D warnings`: 23s
`RUFF_UPDATE_SCHEMA=1 cargo test`: 2min (recompiling), 1min (no code changes, this is mainly doc tests)
`pre-commit run --all-files`: 7s

The exact numbers don't matter so much as the approximate experience (6s is easier to just wait than 1min, esp if you need to fix and rerun). The biggest remaining block seems to be doc tests, i'm surprised i didn't find any solution to speeding them up (nextest simply doesn't run them at all). Also note that the formatter has it's own tests which are much faster since they avoid linking ruff (`cargo test ruff_python_formatter`).

The third change is to enable `cargo test` to update the schema. Similar to `INSTA_UPDATE=always`, i've added `RUFF_UPDATE_SCHEMA=1` (name open to bikeshedding), so `RUFF_UPDATE_SCHEMA=1 cargo test` updates the schema, while `cargo test` still fails as expected if the repo isn't up-to-date.

---

_Comment by @github-actions[bot] on 2023-06-16 12:49_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.06      8.6±0.29ms     4.7 MB/sec    1.00      8.1±0.31ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.01  1701.7±68.84µs     9.8 MB/sec    1.00  1691.1±60.56µs     9.8 MB/sec
formatter/numpy/globals.py                 1.00   167.4±11.43µs    17.6 MB/sec    1.00   167.7±11.76µs    17.6 MB/sec
formatter/pydantic/types.py                1.00      3.3±0.12ms     7.7 MB/sec    1.01      3.4±0.16ms     7.6 MB/sec
linter/all-rules/large/dataset.py          1.00     18.2±0.69ms     2.2 MB/sec    1.04     19.0±0.60ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.12ms     4.0 MB/sec    1.03      4.3±0.15ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   546.7±23.82µs     5.4 MB/sec    1.00   547.8±20.57µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.5±0.25ms     3.4 MB/sec    1.05      7.9±0.27ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.22ms     4.7 MB/sec    1.06      9.1±0.44ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1833.0±63.74µs     9.1 MB/sec    1.00  1830.1±63.20µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.01    218.7±7.87µs    13.5 MB/sec    1.00    217.6±9.84µs    13.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.14ms     6.6 MB/sec    1.02      4.0±0.12ms     6.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.5±0.05ms     5.4 MB/sec    1.01      7.6±0.07ms     5.4 MB/sec
formatter/numpy/ctypeslib.py               1.00  1530.6±22.34µs    10.9 MB/sec    1.01  1540.5±20.60µs    10.8 MB/sec
formatter/numpy/globals.py                 1.00    146.2±2.29µs    20.2 MB/sec    1.02    149.2±5.59µs    19.8 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.04ms     8.3 MB/sec    1.01      3.1±0.04ms     8.2 MB/sec
linter/all-rules/large/dataset.py          1.00     15.3±0.09ms     2.7 MB/sec    1.03     15.8±0.06ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.04ms     4.2 MB/sec    1.02      4.1±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    480.9±6.80µs     6.1 MB/sec    1.01    486.5±5.96µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.05ms     3.8 MB/sec    1.03      6.9±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.05ms     5.1 MB/sec    1.00      8.0±0.04ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1692.6±15.68µs     9.8 MB/sec    1.00  1692.9±18.87µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    192.3±2.27µs    15.3 MB/sec    1.01    194.8±2.66µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.1 MB/sec    1.01      3.6±0.04ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Pre commit with cargo" to "Pre commit without cargo and other pre-PR improvements" by @konstin on 2023-06-16 13:21_

---

_Comment by @konstin on 2023-06-16 14:05_

the benchmarks are really noisy, there are no code changes in the linter or the formatter

---

_Label `internal` added by @konstin on 2023-06-16 14:05_

---

_@dhruvmanila reviewed on 2023-06-17 06:59_

---

_Review comment by @dhruvmanila on `CONTRIBUTING.md`:68 on 2023-06-17 06:59_

I always use this flag to see the actual changes made by the hooks. Do you think it's useful?

```suggestion
pre-commit run --all-files --show-diff-on-failure  # rust and python formatting, markdown and python linting, etc.
```

---

_Review comment by @charliermarsh on `CONTRIBUTING.md`:61 on 2023-06-17 16:07_

Why remove formatting and testing here?

---

_Review comment by @charliermarsh on `crates/ruff_dev/src/generate_json_schema.rs`:66 on 2023-06-17 16:08_

Nit: `env::var("RUFF_UPDATE_SCHEMA").as_ref() == Ok("1")` (no need to allocate)

---

_Review comment by @charliermarsh on `CONTRIBUTING.md`:48 on 2023-06-17 16:09_

Maybe: "And [`pre-commit` ](https://pre-commit.com/#installation), to run some validation checks:"

---

_@charliermarsh approved on 2023-06-17 16:09_

---

_@konstin reviewed on 2023-06-17 19:01_

---

_Review comment by @konstin on `CONTRIBUTING.md`:61 on 2023-06-17 19:01_

formatting is done by pre-commit, testing just moved below clippy

---

_@konstin reviewed on 2023-06-18 10:49_

---

_Review comment by @konstin on `CONTRIBUTING.md`:68 on 2023-06-18 10:49_

this is neat, thanks


---

_Merged by @konstin on 2023-06-18 11:00_

---

_Closed by @konstin on 2023-06-18 11:00_

---

_Branch deleted on 2023-06-18 11:00_

---
