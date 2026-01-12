```yaml
number: 6142
title: Only run unused private type rules over finalized bindings
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/shadowed
created_at: 2023-07-28T01:52:41Z
updated_at: 2023-07-28T02:34:11Z
url: https://github.com/astral-sh/ruff/pull/6142
synced_at: 2026-01-12T15:55:20Z
```

# Only run unused private type rules over finalized bindings

---

_@charliermarsh_

## Summary

In #6134 and #6136, we see some false positives for "shadowed" class definitions. For example, here, the first definition is flagged as unused, since from the perspective of the semantic model (which doesn't understand branching), it appears to be immediately shadowed in the `else`, and thus never used:

```python
if sys.version_info >= (3, 11):
    class _RootLoggerConfiguration(TypedDict, total=False):
        level: _Level
        filters: Sequence[str | _FilterType]
        handlers: Sequence[str]

else:
    class _RootLoggerConfiguration(TypedDict, total=False):
        level: _Level
        filters: Sequence[str]
        handlers: Sequence[str]
```

Instead of looking at _all_ bindings, we should instead look at the "live" bindings, which is similar to how other rules (like unused variables detection) is structured. We thus move the rule from `bindings.rs` (which iterates over _all_ bindings, regardless of whether they're shadowed) to `deferred_scopes.rs`, which iterates over all "live" bindings once a scope has been fully analyzed.

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2023-07-28 02:06_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.6±0.20ms     4.7 MB/sec    1.00      8.6±0.21ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1657.0±25.68µs    10.0 MB/sec    1.00  1653.5±30.84µs    10.1 MB/sec
formatter/numpy/globals.py                 1.00    180.1±1.95µs    16.4 MB/sec    1.00    179.8±0.23µs    16.4 MB/sec
formatter/pydantic/types.py                1.00      3.6±0.05ms     7.1 MB/sec    1.00      3.6±0.07ms     7.1 MB/sec
linter/all-rules/large/dataset.py          1.00     11.1±0.11ms     3.7 MB/sec    1.01     11.2±0.06ms     3.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.00ms     5.9 MB/sec    1.01      2.8±0.01ms     5.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    384.9±0.34µs     7.7 MB/sec    1.01    388.0±0.67µs     7.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.0±0.08ms     5.1 MB/sec    1.03      5.1±0.07ms     5.0 MB/sec
linter/default-rules/large/dataset.py      1.00      5.9±0.02ms     6.8 MB/sec    1.02      6.1±0.03ms     6.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1253.2±2.94µs    13.3 MB/sec    1.01  1263.8±10.50µs    13.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    136.2±0.22µs    21.7 MB/sec    1.02    138.4±1.17µs    21.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.7±0.06ms     9.6 MB/sec    1.00      2.7±0.04ms     9.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     10.4±0.13ms     3.9 MB/sec    1.00     10.2±0.11ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1985.7±23.01µs     8.4 MB/sec    1.00  1983.8±57.52µs     8.4 MB/sec
formatter/numpy/globals.py                 1.00    202.5±2.47µs    14.6 MB/sec    1.01    204.7±5.32µs    14.4 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.05ms     5.8 MB/sec    1.00      4.4±0.06ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.00     13.8±0.19ms     3.0 MB/sec    1.01     14.0±0.17ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.04ms     4.5 MB/sec    1.00      3.7±0.04ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.01   375.1±15.07µs     7.9 MB/sec    1.00    370.1±7.22µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.12ms     4.1 MB/sec    1.00      6.2±0.11ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.7±0.10ms     5.3 MB/sec    1.01      7.8±0.08ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1525.1±26.19µs    10.9 MB/sec    1.00  1526.1±16.91µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    149.9±1.76µs    19.7 MB/sec    1.00    149.7±1.37µs    19.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.03ms     7.6 MB/sec    1.01      3.4±0.03ms     7.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-07-28 02:16_

---

_Closed by @charliermarsh on 2023-07-28 02:16_

---

_Branch deleted on 2023-07-28 02:16_

---
