```yaml
number: 3593
title: Flag PEP 585 and PEP 604 violations in quoted annotations
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/quoted-annotations
created_at: 2023-03-18T03:42:07Z
updated_at: 2023-03-20T15:15:46Z
url: https://github.com/astral-sh/ruff/pull/3593
synced_at: 2026-01-12T15:55:13Z
```

# Flag PEP 585 and PEP 604 violations in quoted annotations

---

_@charliermarsh_

## Summary

At present, we don't flag PEP 585 and PEP 604 errors in quoted annotations. This mirrors `pyupgrade`, which doesn't _fix_ quoted annotations. However, in our case, we should _at least_ be flagging them as errors.

As a reminder, PEP 585 introduced standard library generics, so you can do `list[int]` instead of `from typing import List; List[int]`. PEP 604 added new syntax for unions, so you can do `str | int` instead of `from typing import Union; Union[str, int]`.

Note that we _can_ fix simple quoted annotations, but... we need to take care in certain cases.

For example: a quoted annotation can contain an implicit string concatenation, like `x: "Li" "st[int] = []`. This is also valid:

```py
x: """
List[str]
""" = []
```

Fixing those cases will be challenging, so for now, it's safer just to avoid fixing them at all (though we should add autofix for "simple" cases soon).

Closes #3555.


---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-18 03:42_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-18 03:42_

---

_Review requested from @konstin by @charliermarsh on 2023-03-18 03:42_

---

_Unassigned @charliermarsh by @charliermarsh on 2023-03-18 03:42_

---

_Comment by @github-actions[bot] on 2023-03-18 03:55_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.4±0.08ms     3.0 MB/sec    1.00     13.4±0.08ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    479.4±0.94µs     6.2 MB/sec    1.01    482.8±1.02µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.02ms     4.3 MB/sec    1.00      5.9±0.02ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.02ms     5.7 MB/sec    1.00      7.1±0.02ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1604.0±1.56µs    10.4 MB/sec    1.00   1604.8±1.38µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    180.0±0.35µs    16.4 MB/sec    1.00    180.2±0.42µs    16.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.01ms     7.6 MB/sec    1.00      3.4±0.01ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.8±0.19ms     2.7 MB/sec    1.01     15.0±0.21ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.06ms     4.1 MB/sec    1.01      4.1±0.07ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   453.0±24.56µs     6.5 MB/sec    1.01   458.5±33.09µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.09ms     3.8 MB/sec    1.01      6.7±0.10ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.09ms     5.0 MB/sec    1.01      8.2±0.07ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1746.5±21.26µs     9.5 MB/sec    1.01  1768.3±32.04µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    180.6±4.35µs    16.3 MB/sec    1.02    183.6±7.80µs    16.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.06ms     6.7 MB/sec    1.02      3.9±0.06ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:2174 on 2023-03-20 08:18_

My understanding is that our guideline is to use rule names that work well with `allow`. Is this an internal-only name and what's the motivation of using the PEP names?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pyupgrade/rules/use_pep585_annotation.rs`:17 on 2023-03-20 08:22_

NIt: Unrelated to this change: A `Availability::None` would remove the need for `Option<AutofixKind>`. It would then simply be `AUTOFX: AutofixKind = AutofixKind::NONE` (we could even inline `Availability` into `AutofixKind` 

---

_@MichaReiser approved on 2023-03-20 08:23_

---

_@konstin reviewed on 2023-03-20 09:47_

---

_Review comment by @konstin on `crates/ruff/src/checkers/ast/mod.rs`:2174 on 2023-03-20 09:47_

python features often lack a name beyond the PEP number. e.g. https://docs.python.org/3/library/typing.html says:

> [PEP 604](https://peps.python.org/pep-0604/): Allow writing union types as X | Y
> Introducing [types.UnionType](https://docs.python.org/3/library/types.html#types.UnionType) and the ability to use the binary-or operator | to signify a [union of types](https://docs.python.org/3/library/stdtypes.html#types-union)

we could invent our own name such as `NonPipeUnion`.

---

_@konstin approved on 2023-03-20 09:51_

---

_@charliermarsh reviewed on 2023-03-20 15:15_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/rules/use_pep585_annotation.rs`:17 on 2023-03-20 15:15_

Yeah this would be a good change, I think.

---

_Merged by @charliermarsh on 2023-03-20 15:15_

---

_Closed by @charliermarsh on 2023-03-20 15:15_

---

_Branch deleted on 2023-03-20 15:15_

---
