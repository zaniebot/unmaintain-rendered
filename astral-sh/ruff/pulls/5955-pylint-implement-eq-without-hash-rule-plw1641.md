```yaml
number: 5955
title: "[pylint] Implement `eq-without-hash` rule (PLW1641)"
type: pull_request
state: merged
author: jelly
labels:
  - rule
assignees: []
merged: true
base: main
head: pylint-eq-without-hash
created_at: 2023-07-21T21:15:39Z
updated_at: 2023-07-27T19:07:43Z
url: https://github.com/astral-sh/ruff/pull/5955
synced_at: 2026-01-12T15:55:20Z
```

# [pylint] Implement `eq-without-hash` rule (PLW1641)

---

_@jelly_

Implement https://pylint.pycqa.org/en/latest/user_guide/messages/warning/eq-without-hash.html
Issue https://github.com/astral-sh/ruff/issues/970

It's not enabled by default in pylint, so I guess it shouldn't in Ruff either?

---

_Comment by @github-actions[bot] on 2023-07-21 21:39_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      8.9±0.05ms     4.6 MB/sec    1.01      9.0±0.03ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.00   1728.0±5.46µs     9.6 MB/sec    1.00   1728.7±6.89µs     9.6 MB/sec
formatter/numpy/globals.py                 1.00    181.8±0.88µs    16.2 MB/sec    1.01    183.9±1.15µs    16.0 MB/sec
formatter/pydantic/types.py                1.00      3.8±0.01ms     6.7 MB/sec    1.00      3.8±0.01ms     6.7 MB/sec
linter/all-rules/large/dataset.py          1.01     11.7±0.06ms     3.5 MB/sec    1.00     11.6±0.03ms     3.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.0±0.01ms     5.6 MB/sec    1.00      3.0±0.02ms     5.5 MB/sec
linter/all-rules/numpy/globals.py          1.03    329.5±0.75µs     9.0 MB/sec    1.00    318.9±0.75µs     9.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.3±0.02ms     4.9 MB/sec    1.00      5.2±0.01ms     4.9 MB/sec
linter/default-rules/large/dataset.py      1.00      6.5±0.02ms     6.3 MB/sec    1.00      6.4±0.01ms     6.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1287.8±2.27µs    12.9 MB/sec    1.00   1286.9±3.50µs    12.9 MB/sec
linter/default-rules/numpy/globals.py      1.01    127.3±0.24µs    23.2 MB/sec    1.00    126.7±0.84µs    23.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.8±0.03ms     9.1 MB/sec    1.00      2.8±0.02ms     9.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.1±0.11ms     4.0 MB/sec    1.00     10.1±0.10ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.01  1968.5±25.06µs     8.5 MB/sec    1.00  1953.8±34.63µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00    215.8±4.16µs    13.7 MB/sec    1.02   219.8±10.54µs    13.4 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.05ms     6.0 MB/sec    1.01      4.3±0.07ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.5±0.11ms     3.0 MB/sec    1.01     13.6±0.12ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.03ms     4.7 MB/sec    1.00      3.5±0.03ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    428.5±6.38µs     6.9 MB/sec    1.00    430.0±5.46µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.07ms     4.2 MB/sec    1.00      6.1±0.06ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.06ms     5.4 MB/sec    1.00      7.5±0.05ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1514.3±25.47µs    11.0 MB/sec    1.00  1506.1±16.43µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    166.2±2.51µs    17.8 MB/sec    1.01    168.0±3.50µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.03ms     7.8 MB/sec    1.01      3.3±0.05ms     7.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@sbrugman reviewed on 2023-07-23 01:24_

---

_Review comment by @sbrugman on `crates/ruff/src/rules/pylint/rules/eq_without_hash.rs`:16 on 2023-07-23 01:24_

Implementing `__eq__` sets `__hash__` to `None` unless implemented:

> A class that overrides [__eq__()](https://docs.python.org/3/reference/datamodel.html#object.__eq__) and does not define [__hash__()](https://docs.python.org/3/reference/datamodel.html#object.__hash__) will have its [__hash__()](https://docs.python.org/3/reference/datamodel.html#object.__hash__) implicitly set to None.


---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/eq_without_hash.rs`:60 on 2023-07-24 07:11_

```suggestion
fn has_eq_without_hash(body: &[Stmt]) -> bool {
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/eq_without_hash.rs`:72 on 2023-07-24 07:13_

```suggestion
        if name == "__hash__" {
            has_hash = true;
        } else if name == "__eq__" {
            has_eq = true;
        }
```

or

```suggestion
		match name {
			"__hash__" => {
				has_hash = true
			},
			"__eq__" => {
				has_eq => true 
			}
			_ => {}
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pylint/rules/eq_without_hash.rs`:76 on 2023-07-24 07:15_

I wonder if it would make sense performance-wise to early-exit if we've seen both methods, which should be the case for code that has no violation. 

Up: Potentially fewer iterations
Down: More branching in a hot loop, may result in more branch misses.

---

_@MichaReiser approved on 2023-07-24 07:15_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-07-25 07:00_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-27 13:55_

---

_Label `rule` added by @charliermarsh on 2023-07-27 18:21_

---

_Merged by @charliermarsh on 2023-07-27 18:28_

---

_Closed by @charliermarsh on 2023-07-27 18:28_

---

_Branch deleted on 2023-07-27 19:07_

---
