```yaml
number: 4764
title: "Add PYI034 for `flake8-pyi` plugin "
type: pull_request
state: merged
author: qdegraaf
labels:
  - rule
assignees: []
merged: true
base: main
head: feature/PYI034
created_at: 2023-05-31T19:07:23Z
updated_at: 2023-06-02T14:43:14Z
url: https://github.com/astral-sh/ruff/pull/4764
synced_at: 2026-01-12T15:55:16Z
```

# Add PYI034 for `flake8-pyi` plugin 

---

_@qdegraaf_

## Summary

Add [Y034](https://github.com/PyCQA/flake8-pyi/blob/ceab86d16b822d12abae1d8087850d0bc66d2f52/README.md?plain=1#L69) from [flake8-pyi](https://github.com/PyCQA/flake8-pyi) to the plugin. 

## Test Plan

Added test fixtures based on cases from Flake8-Pyi repo

## Issue links

Refers: https://github.com/charliermarsh/ruff/issues/848


---

_Marked ready for review by @qdegraaf on 2023-05-31 19:29_

---

_Comment by @github-actions[bot] on 2023-05-31 19:58_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+4, -0, 0 error(s))

<details><summary>typeshed (+4, -0)</summary>
<p>

```diff
+ stdlib/typing_extensions.pyi:165:13: PYI034 `__ior__` methods in classes like `_TypedDict` usually return `self` at runtime
+ stubs/SQLAlchemy/sqlalchemy/util/langhelpers.pyi:136:9: PYI034 `__new__` methods in classes like `_symbol` usually return `self` at runtime
+ stubs/pytz/pytz/lazy.pyi:13:9: PYI034 `__new__` methods in classes like `LazyList` usually return `self` at runtime
+ stubs/pytz/pytz/lazy.pyi:17:9: PYI034 `__new__` methods in classes like `LazySet` usually return `self` at runtime
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PYI034 | 4 | 4 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     19.5±0.50ms     2.1 MB/sec    1.00     19.3±0.62ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.5±0.12ms     3.7 MB/sec    1.02      4.6±0.14ms     3.6 MB/sec
linter/all-rules/numpy/globals.py          1.00   579.9±17.92µs     5.1 MB/sec    1.00   578.4±22.41µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.9±0.26ms     3.2 MB/sec    1.00      7.8±0.27ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.01      9.1±0.23ms     4.5 MB/sec    1.00      8.9±0.31ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02      2.0±0.06ms     8.2 MB/sec    1.00  1989.5±59.22µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.03   246.9±15.21µs    11.9 MB/sec    1.00   240.3±14.70µs    12.3 MB/sec
linter/default-rules/pydantic/types.py     1.02      4.2±0.15ms     6.0 MB/sec    1.00      4.1±0.17ms     6.2 MB/sec
parser/large/dataset.py                    1.00      7.0±0.16ms     5.8 MB/sec    1.04      7.3±0.27ms     5.6 MB/sec
parser/numpy/ctypeslib.py                  1.00  1386.8±29.36µs    12.0 MB/sec    1.03  1430.9±36.86µs    11.6 MB/sec
parser/numpy/globals.py                    1.00    137.4±5.61µs    21.5 MB/sec    1.06    145.9±3.80µs    20.2 MB/sec
parser/pydantic/types.py                   1.00      3.1±0.07ms     8.3 MB/sec    1.03      3.1±0.14ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.0±0.11ms     2.4 MB/sec    1.00     17.0±0.12ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.4±0.04ms     3.8 MB/sec    1.00      4.4±0.04ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    452.3±9.56µs     6.5 MB/sec    1.00    452.7±9.45µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.05ms     3.5 MB/sec    1.00      7.3±0.07ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.6±0.04ms     4.7 MB/sec    1.01      8.7±0.09ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1800.3±12.43µs     9.2 MB/sec    1.00  1805.9±10.24µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    198.4±1.47µs    14.9 MB/sec    1.01    200.9±3.10µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.04ms     6.5 MB/sec    1.00      3.9±0.04ms     6.5 MB/sec
parser/large/dataset.py                    1.00      6.7±0.02ms     6.1 MB/sec    1.00      6.7±0.02ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00   1261.9±9.01µs    13.2 MB/sec    1.00   1265.9±6.84µs    13.2 MB/sec
parser/numpy/globals.py                    1.00    130.7±0.75µs    22.6 MB/sec    1.01    132.1±0.90µs    22.3 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.01ms     9.0 MB/sec    1.00      2.8±0.02ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @charliermarsh by @charliermarsh on 2023-06-01 04:43_

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:749 on 2023-06-01 06:25_

Nit: you can reduce the number of arguments by passing the entire class:

1. Change the match arm on line 655 to

```rust
Stmt::ClassDef(class @ ast::StmtClassDef {
                name,
                bases,
                keywords,
                decorator_list,
                body,
                range: _,
}) => {
```

then pass it here 
```suggestion
                        flake8_pyi::rules::fixed_return_type(self, class);
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_pyi/rules/fixed_return_type.rs`:158 on 2023-06-01 06:28_

Nit: I find this easier to read because I don't have to look for where the variable is mutated (I can read top down rather than having to search for the mutations)
```suggestion
    let is_iter_subclass = if bases.len() == 1 {
        let base = if let Expr::Subscript(ast::ExprSubscript { value, .. }) = &bases[0] {
            // Ex) class Foo(Iterator[T]):
            value
        } else {
            // Ex) class Foo(Iterator):
            &bases[0]
        };
        checker
            .semantic_model()
            .resolve_call_path(base)
            .map_or(false, |call_path| {
                ITERATOR_BASES.contains(&call_path.as_slice())
            });
    } else {
    	false
  	}
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_pyi/rules/fixed_return_type.rs`:186 on 2023-06-01 06:29_

Nit: It could be worth to move this above line 177 because this is a very cheap check compared to performing any operations on the semantic model.

---

_@MichaReiser approved on 2023-06-01 06:30_

---

_Label `rule` added by @charliermarsh on 2023-06-02 01:57_

---

_Merged by @charliermarsh on 2023-06-02 02:15_

---

_Closed by @charliermarsh on 2023-06-02 02:15_

---

_Branch deleted on 2023-06-02 14:43_

---
