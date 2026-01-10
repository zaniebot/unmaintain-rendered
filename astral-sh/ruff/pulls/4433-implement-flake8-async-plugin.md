```yaml
number: 4433
title: "Implement `flake8-async` plugin"
type: pull_request
state: closed
author: inikolaev
labels: []
assignees: []
draft: true
base: main
head: inikolaev/4246-implement-flake8-async-plugin
created_at: 2023-05-14T18:58:07Z
updated_at: 2024-12-29T17:57:14Z
url: https://github.com/astral-sh/ruff/pull/4433
synced_at: 2026-01-10T20:42:26Z
```

# Implement `flake8-async` plugin

---

_Pull request opened by @inikolaev on 2023-05-14 18:58_

Added implementation of [flake8-async](https://github.com/cooperlees/flake8-async) plugin and copied all the tests from it into `ruff`.

TODOs:
- [ ] Add some descriptions to the new violations

---

_@inikolaev reviewed on 2023-05-14 19:00_

---

_Review comment by @inikolaev on `crates/ruff/src/checkers/ast/mod.rs`:601 on 2023-05-14 19:00_

This whole match `branch` is being applied to both sync and async functions, but these new checks should only apply to `async`. Not sure if there's some other way to do the same.

---

_@inikolaev reviewed on 2023-05-14 19:01_

---

_Review comment by @inikolaev on `crates/ruff/src/rules/flake8_async/mod.rs`:17 on 2023-05-14 19:01_

Not really sure if 3 different test cases are really necessary, I just followed other plugins. A single test case that tests all of them would work as well - all examples are functions with 1-2 lines.

---

_@inikolaev reviewed on 2023-05-14 19:02_

---

_Review comment by @inikolaev on `crates/ruff/src/rules/flake8_async/rules.rs`:89 on 2023-05-14 19:02_

Copied from the original plugin

---

_@inikolaev reviewed on 2023-05-14 19:03_

---

_Review comment by @inikolaev on `crates/ruff/src/rules/flake8_async/rules.rs`:127 on 2023-05-14 19:03_

Quite straightforward implementation, happy to hear any feedback

---

_@JonathanPlasse reviewed on 2023-05-14 19:05_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/checkers/ast/mod.rs`:601 on 2023-05-14 19:05_

```suggestion
                // Rules that apply to async functions only
                if stmt.is_async_function_def_stmt() {
```

---

_Comment by @mara004 on 2023-05-14 19:53_

Looks like #4432 was submitted simultaneously?

---

_Comment by @github-actions[bot] on 2023-05-14 20:16_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.0±0.15ms     2.5 MB/sec    1.02     16.3±0.15ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.04ms     4.2 MB/sec    1.00      4.0±0.08ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.01    487.8±8.17µs     6.0 MB/sec    1.00    482.7±6.04µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.06ms     3.8 MB/sec    1.00      6.8±0.09ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.06ms     5.1 MB/sec    1.04      8.3±0.07ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1724.1±16.14µs     9.7 MB/sec    1.02  1765.0±19.90µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    189.6±3.35µs    15.6 MB/sec    1.03    195.5±2.25µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.0 MB/sec    1.03      3.8±0.04ms     6.8 MB/sec
parser/large/dataset.py                    1.01      6.3±0.07ms     6.5 MB/sec    1.00      6.2±0.05ms     6.6 MB/sec
parser/numpy/ctypeslib.py                  1.00  1205.9±17.70µs    13.8 MB/sec    1.00  1201.7±15.94µs    13.9 MB/sec
parser/numpy/globals.py                    1.01    123.4±1.58µs    23.9 MB/sec    1.00    122.0±1.72µs    24.2 MB/sec
parser/pydantic/types.py                   1.00      2.6±0.03ms     9.7 MB/sec    1.00      2.6±0.03ms     9.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.03     16.6±0.14ms     2.5 MB/sec    1.00     16.1±0.10ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.2±0.06ms     3.9 MB/sec    1.00      4.2±0.05ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.03    501.5±7.65µs     5.9 MB/sec    1.00    487.9±7.98µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.0±0.06ms     3.7 MB/sec    1.00      6.8±0.06ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.1±0.05ms     5.0 MB/sec    1.00      8.1±0.08ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1742.6±27.00µs     9.6 MB/sec    1.00  1701.5±14.52µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.03    198.1±3.76µs    14.9 MB/sec    1.00    192.8±3.59µs    15.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.03ms     6.9 MB/sec    1.00      3.6±0.07ms     7.0 MB/sec
parser/large/dataset.py                    1.00      6.6±0.04ms     6.2 MB/sec    1.01      6.7±0.04ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1261.2±13.35µs    13.2 MB/sec    1.01  1272.9±13.87µs    13.1 MB/sec
parser/numpy/globals.py                    1.00    129.6±3.56µs    22.8 MB/sec    1.00    129.7±1.55µs    22.7 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.1 MB/sec    1.01      2.8±0.02ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Closed by @inikolaev on 2023-05-15 07:44_

---

_Branch deleted on 2024-12-29 17:57_

---
