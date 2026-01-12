```yaml
number: 6696
title: "Use `typing_extensions.TypeAlias` for PYI026 fixes on pre-3.10"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/typing
created_at: 2023-08-19T21:39:37Z
updated_at: 2023-08-19T22:45:26Z
url: https://github.com/astral-sh/ruff/pull/6696
synced_at: 2026-01-12T02:52:04Z
```

# Use `typing_extensions.TypeAlias` for PYI026 fixes on pre-3.10

---

_Pull request opened by @charliermarsh on 2023-08-19 21:39_

Closes https://github.com/astral-sh/ruff/issues/6695.

---

_Label `bug` added by @charliermarsh on 2023-08-19 21:43_

---

_@Skylion007 reviewed on 2023-08-19 21:48_

---

_Review comment by @Skylion007 on `crates/ruff/src/rules/flake8_pyi/rules/mod.rs`:82 on 2023-08-19 21:48_

I wonder if it would be more useful to generalize this into a generic backport import helper. 

IE. something that would automatically downgrade typing.TypeAlias. Alternatively, I wonder if you could use the pyupgrade import rules logic here and always apply that rule, but only to this specific import. It would either be a NOOP or automatically fix the import if the import was moved to the typing module.

That also has the added benefit of only having to maintain the version when a new import was introduced in one place (the pyupgrade rules).

---

_@Skylion007 reviewed on 2023-08-19 21:49_

---

_Review comment by @Skylion007 on `crates/ruff/src/rules/flake8_pyi/rules/mod.rs`:82 on 2023-08-19 21:49_

The logic could even be folded into the helper which inserts the import if you do it pyupgrade way.

---

_@Skylion007 reviewed on 2023-08-19 21:54_

---

_Review comment by @Skylion007 on `crates/ruff/src/rules/flake8_pyi/rules/mod.rs`:82 on 2023-08-19 21:54_

```
Note that, in some cases, it may be preferable to continue importing members from typing_extensions even after they're added to the Python standard library, as typing_extensions can backport bugfixes and optimizations from later Python versions. This rule thus avoids flagging imports from typing_extensions in such cases.
```
Seems like more reason to use the helper from this rule here: https://beta.ruff.rs/docs/rules/deprecated-import/

---

_@charliermarsh reviewed on 2023-08-19 21:55_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/mod.rs`:82 on 2023-08-19 21:55_

Nice idea, I will look into it separately. There are a few other places where this would be useful, like `PYI019` which suggests `typing.Self`.

---

_Comment by @github-actions[bot] on 2023-08-19 21:57_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -2, 0 error(s))

<details><summary>typeshed (+0, -2)</summary>
<p>

<pre>
- <a href='https://github.com/python/typeshed/blob/6a8d653a671925b0a3af61729ff8cf3f90c9c662/stdlib/builtins.pyi#L1666'>stdlib/builtins.pyi:1666:1:</a> PYI026 Use `typing.TypeAlias` for type alias, e.g., `_SupportsSomeKindOfPow: typing.TypeAlias = _SupportsPow2[Any, Any] | _SupportsPow3NoneOnly[Any, Any] | _SupportsPow3[Any, Any, Any]`
- <a href='https://github.com/python/typeshed/blob/6a8d653a671925b0a3af61729ff8cf3f90c9c662/stdlib/builtins.pyi#L218'>stdlib/builtins.pyi:218:1:</a> PYI026 Use `typing.TypeAlias` for type alias, e.g., `_LiteralInteger: typing.TypeAlias = _PositiveInteger | _NegativeInteger | Literal[0]`
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PYI026 | 2 | 0 | 2 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      3.9±0.02ms    10.4 MB/sec    1.00      3.9±0.02ms    10.5 MB/sec
formatter/numpy/ctypeslib.py               1.01    821.1±3.88µs    20.3 MB/sec    1.00    815.5±3.47µs    20.4 MB/sec
formatter/numpy/globals.py                 1.00     85.7±0.46µs    34.4 MB/sec    1.00     85.4±0.42µs    34.6 MB/sec
formatter/pydantic/types.py                1.04  1654.8±54.56µs    15.4 MB/sec    1.00  1589.8±18.72µs    16.0 MB/sec
linter/all-rules/large/dataset.py          1.00     12.2±0.04ms     3.3 MB/sec    1.02     12.5±0.39ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.1 MB/sec    1.00      3.3±0.04ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.04   484.5±27.63µs     6.1 MB/sec    1.00    465.6±1.25µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.4±0.03ms     4.0 MB/sec    1.00      6.3±0.02ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      6.5±0.02ms     6.2 MB/sec    1.00      6.5±0.04ms     6.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1450.3±21.12µs    11.5 MB/sec    1.00  1433.9±13.44µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.02    171.1±6.06µs    17.2 MB/sec    1.00    168.0±2.16µs    17.6 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.0±0.01ms     8.6 MB/sec    1.00      2.9±0.04ms     8.7 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      4.7±0.21ms     8.7 MB/sec     1.00      4.7±0.24ms     8.7 MB/sec
formatter/numpy/ctypeslib.py               1.00   972.0±41.74µs    17.1 MB/sec     1.00   975.2±41.60µs    17.1 MB/sec
formatter/numpy/globals.py                 1.00     98.4±5.11µs    30.0 MB/sec     1.06   104.7±17.17µs    28.2 MB/sec
formatter/pydantic/types.py                1.01  1970.1±172.06µs    12.9 MB/sec    1.00  1959.3±123.97µs    13.0 MB/sec
linter/all-rules/large/dataset.py          1.02     16.4±0.52ms     2.5 MB/sec     1.00     16.1±0.51ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.5±0.19ms     3.7 MB/sec     1.00      4.4±0.19ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   552.5±21.74µs     5.3 MB/sec     1.04   575.6±38.97µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.4±0.31ms     3.0 MB/sec     1.00      8.4±0.37ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.01      9.0±0.24ms     4.5 MB/sec     1.00      8.9±0.30ms     4.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1901.9±75.79µs     8.8 MB/sec     1.01  1911.8±63.80µs     8.7 MB/sec
linter/default-rules/numpy/globals.py      1.00   220.1±11.69µs    13.4 MB/sec     1.02   224.9±15.96µs    13.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.0±0.26ms     6.3 MB/sec     1.01      4.1±0.26ms     6.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-08-19 22:04_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/mod.rs`:82 on 2023-08-19 22:04_

Yup agreed. (I did check if that applied to `TypeAlias` and it does not.)


---

_@charliermarsh reviewed on 2023-08-19 22:07_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/mod.rs`:82 on 2023-08-19 22:07_

Adding a TODO, I may look into it tonight.

---

_Merged by @charliermarsh on 2023-08-19 22:16_

---

_Closed by @charliermarsh on 2023-08-19 22:16_

---

_Branch deleted on 2023-08-19 22:16_

---
