```yaml
number: 4888
title: Track symbol deletions separately from bindings
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/declares
created_at: 2023-06-06T01:29:57Z
updated_at: 2023-06-06T19:07:02Z
url: https://github.com/astral-sh/ruff/pull/4888
synced_at: 2026-01-12T03:43:29Z
```

# Track symbol deletions separately from bindings

---

_Pull request opened by @charliermarsh on 2023-06-06 01:29_

## Summary

#4746 revealed some subtle issues related to Python's handling of `nonlocal`. If you use `nonlocal`, and the non-local symbol isn't declared in the parent scope, Python raises a syntax error at parse time (!). However, I didn't realize that, for the purpose of declarations, Python will respect a `del` statement -- so this works fine:

```py
def f():
    def g():
        nonlocal x

    del x
```

Previously, to determine whether the symbol was declared in the parent scope, we checked if the symbol was _bound_ in the parent scope. But we don't create bindings for deletions, and in fact, we remove any existing bindings when we hit a deletion.

This PR thus modifies `Scope` to track all deleted symbols, so that we can ask the scope whether the symbol should be considered "declared" for the purpose of `nonlocal` analysis.

Closes #4746.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-06-06 01:30_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/scope.rs`:27 on 2023-06-06 01:30_

This might be a bit of a heavy-handed change just to fix this one semantic issue... E.g., we could instead track `deleted_names`, and make `declares` something like `self.has(name) || self.deletions.contains(name)`.

---

_@charliermarsh reviewed on 2023-06-06 01:30_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/scope.rs`:27 on 2023-06-06 01:32_

(We could also track `HashMap<&'a str, SymbolId>`, and make `bindings: FxHashMap<SymbolId, BindingId>`.)

---

_@charliermarsh reviewed on 2023-06-06 01:32_

---

_Comment by @github-actions[bot] on 2023-06-06 02:08_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -1, 0 error(s))

<details><summary>airflow (+0, -1)</summary>
<p>

```diff
- tests/decorators/test_task_group.py:108:18: PLE0117 Nonlocal name `tgp` found without binding
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PLE0117 | 1 | 0 | 1 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.10      6.5±0.02ms     6.3 MB/sec    1.00      5.9±0.02ms     6.9 MB/sec
formatter/numpy/ctypeslib.py               1.07   1297.3±1.98µs    12.8 MB/sec    1.00   1211.6±2.78µs    13.7 MB/sec
formatter/numpy/globals.py                 1.05    143.7±3.88µs    20.5 MB/sec    1.00    137.0±0.29µs    21.5 MB/sec
formatter/pydantic/types.py                1.08      2.8±0.00ms     9.1 MB/sec    1.00      2.6±0.01ms     9.8 MB/sec
linter/all-rules/large/dataset.py          1.01     14.1±0.07ms     2.9 MB/sec    1.00     13.9±0.06ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     5.0 MB/sec    1.00      3.4±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    413.7±0.78µs     7.1 MB/sec    1.00    414.4±0.84µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.03ms     4.3 MB/sec    1.00      5.9±0.03ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.02ms     6.0 MB/sec    1.00      6.8±0.01ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1468.7±4.74µs    11.3 MB/sec    1.00   1461.3±4.81µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.01    163.5±0.44µs    18.0 MB/sec    1.00    161.7±0.25µs    18.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.00ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.6±0.27ms     4.7 MB/sec    1.00      8.5±0.24ms     4.8 MB/sec
formatter/numpy/ctypeslib.py               1.00  1732.9±63.90µs     9.6 MB/sec    1.01  1753.6±73.40µs     9.5 MB/sec
formatter/numpy/globals.py                 1.00    196.5±9.47µs    15.0 MB/sec    1.00   196.7±13.69µs    15.0 MB/sec
formatter/pydantic/types.py                1.00      3.8±0.10ms     6.8 MB/sec    1.01      3.8±0.15ms     6.8 MB/sec
linter/all-rules/large/dataset.py          1.01     20.6±0.49ms  2018.8 KB/sec    1.00     20.4±0.39ms  2038.6 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.14ms     3.3 MB/sec    1.00      5.1±0.17ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.02   604.7±24.19µs     4.9 MB/sec    1.00   595.1±22.40µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.6±0.24ms     3.0 MB/sec    1.00      8.6±0.27ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00     10.1±0.23ms     4.0 MB/sec    1.00     10.1±0.25ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.2±0.18ms     7.7 MB/sec    1.00      2.1±0.08ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.01   243.5±11.40µs    12.1 MB/sec    1.00   241.9±12.28µs    12.2 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.6±0.24ms     5.5 MB/sec    1.00      4.5±0.16ms     5.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@Boshen reviewed on 2023-06-06 06:41_

---

_Review comment by @Boshen on `crates/ruff_python_semantic/src/scope.rs`:27 on 2023-06-06 06:41_

I'm having a hard time comprehending the difference between a `declared_symbol` vs a `binding` based on their names and comments alone 🤯 

---

_@konstin approved on 2023-06-06 07:47_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/scope.rs`:27 on 2023-06-06 13:39_

I'm going to rewrite this to special-case deletions. I need to reconsider deletions more holistically.

---

_@charliermarsh reviewed on 2023-06-06 13:39_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/scope.rs`:77 on 2023-06-06 15:26_

Would it make sense to instead *mark* the binding as deleted? I assume that causes some downstream problems because a lot of code freely access the bindings directly, so there isn't a good way for us to hide deleted bindings. I'm mainly asking because I'm a bit worried about registering all bindings in two separate `HashMaps`.

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/scope.rs`:23 on 2023-06-06 15:29_

How is `declared_symbols` different from `bindings`?

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/scope.rs`:23 on 2023-06-06 15:30_

Would it make sense to instead track deleted `symbol`s, considering that deletions are rare (so that the vec is empty 99% of the cases)?

---

_@MichaReiser reviewed on 2023-06-06 15:30_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/scope.rs`:23 on 2023-06-06 16:56_

Yes, I did that initially. I'm going to revert to that state.

---

_@charliermarsh reviewed on 2023-06-06 16:56_

---

_@charliermarsh reviewed on 2023-06-06 16:56_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/scope.rs`:23 on 2023-06-06 16:56_

This is defined on the relevant method docstrings (`declares` vs. `has`).

---

_@charliermarsh reviewed on 2023-06-06 16:57_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/scope.rs`:77 on 2023-06-06 16:57_

This actually isn't sufficient, because this needs to be considered valid (for the purpose of the `nonlocal` rule):

```py
def f():
    del x

    def g():
        nonlocal x
```

`del x` adds `x` to the symbol table, but doesn't bind it to the value (or something like that -- I don't have the terminology nailed down).

---

_Renamed from "Track symbol declarations separately from bindings" to "Track symbol deletions separately from bindings" by @charliermarsh on 2023-06-06 18:39_

---

_Merged by @charliermarsh on 2023-06-06 18:49_

---

_Closed by @charliermarsh on 2023-06-06 18:49_

---

_Branch deleted on 2023-06-06 18:49_

---
