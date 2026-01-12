```yaml
number: 6501
title: Use one line between top-level items if formatting a stub file
type: pull_request
state: merged
author: tjkuson
labels:
  - formatter
assignees: []
merged: true
base: main
head: single-line-stubs
created_at: 2023-08-11T13:01:29Z
updated_at: 2023-08-15T09:09:04Z
url: https://github.com/astral-sh/ruff/pull/6501
synced_at: 2026-01-12T15:55:21Z
```

# Use one line between top-level items if formatting a stub file

---

_@tjkuson_

## Summary

When formatting stub files, use only one line between top-level items. Use zero lines between class definitions of the same type that have an empty body (nothing but `...`). Closes #5821.

This PR doesn't implement moving the ellipsis to the same line as the class definition signature. I have never touched the formatter code before, so was looking around about how to do it and found #5822. Seeing as it has its own issue, I might just leave it out of this PR.

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2023-08-11 13:34_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.9±0.02ms    10.4 MB/sec    1.00      3.9±0.03ms    10.4 MB/sec
formatter/numpy/ctypeslib.py               1.00    780.1±3.25µs    21.3 MB/sec    1.00    780.8±3.16µs    21.3 MB/sec
formatter/numpy/globals.py                 1.00     80.1±0.37µs    36.8 MB/sec    1.01     80.8±0.69µs    36.5 MB/sec
formatter/pydantic/types.py                1.00   1563.9±6.25µs    16.3 MB/sec    1.00   1562.4±3.42µs    16.3 MB/sec
linter/all-rules/large/dataset.py          1.00     10.3±0.02ms     4.0 MB/sec    1.01     10.4±0.01ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.01ms     6.0 MB/sec    1.01      2.8±0.00ms     6.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    394.8±0.48µs     7.5 MB/sec    1.00    395.4±0.59µs     7.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.3±0.01ms     4.8 MB/sec    1.01      5.4±0.01ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.00      5.5±0.01ms     7.5 MB/sec    1.01      5.5±0.02ms     7.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1206.4±1.11µs    13.8 MB/sec    1.00   1207.8±5.27µs    13.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    140.1±0.25µs    21.1 MB/sec    1.01    141.2±1.11µs    20.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.00ms    10.3 MB/sec    1.00      2.5±0.01ms    10.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.3±0.06ms     9.5 MB/sec    1.00      4.3±0.06ms     9.5 MB/sec
formatter/numpy/ctypeslib.py               1.00   823.3±11.00µs    20.2 MB/sec    1.00   827.4±10.63µs    20.1 MB/sec
formatter/numpy/globals.py                 1.00     82.5±1.26µs    35.8 MB/sec    1.01     83.5±1.87µs    35.3 MB/sec
formatter/pydantic/types.py                1.00  1695.2±29.73µs    15.0 MB/sec    1.01  1710.9±48.78µs    14.9 MB/sec
linter/all-rules/large/dataset.py          1.00     12.5±0.15ms     3.3 MB/sec    1.02     12.8±0.19ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.03ms     4.9 MB/sec    1.02      3.5±0.02ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    424.6±7.44µs     6.9 MB/sec    1.03    438.7±5.69µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.4±0.06ms     4.0 MB/sec    1.03      6.6±0.09ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.07ms     5.9 MB/sec    1.06      7.2±0.20ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1437.1±14.35µs    11.6 MB/sec    1.04  1487.7±20.53µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    166.6±5.67µs    17.7 MB/sec    1.04    172.8±3.69µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.05ms     8.3 MB/sec    1.03      3.1±0.03ms     8.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Renamed from "Use one line betweeen top level items if in stub file" to "Use one line between top-level items if formatting a stub file" by @tjkuson on 2023-08-11 15:38_

---

_Marked ready for review by @tjkuson on 2023-08-11 15:40_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:245 on 2023-08-11 17:25_

```suggestion
fn contains_only_an_ellipsis(body: &[Stmt]) -> bool {
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:258 on 2023-08-11 17:26_

Nit

```suggestion
match body {
	[Stmt::Expr(ast::StmtExpr { value, ..})] => matches!(
	        value.as_ref(),
	        Expr::Constant(ast::ExprConstant {
	            value: Constant::Ellipsis,
	            ..
	        })
	  ),
	  _ => false
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/top_level.pyi`:24 on 2023-08-11 17:28_

Can we add a few tests where the classes have interleaved comments:

```python
class Del(expr_context):
	...

# comment
class Other(expr_context):
	...
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:142 on 2023-08-11 17:29_

This seems very particular. Does Black really compare the name of the arguments? Do we also need to compare the type parameters?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:144 on 2023-08-11 17:30_

Nit: It could make sense to move this check above the `args` check because it is relatively cheap (compared with zipping and comparing all arguments)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/fixtures.rs`:164 on 2023-08-11 17:31_

Would this work too
```suggestion
    insta::glob!("../resources", "test/fixtures/ruff/**/*.{py,pyi}", test_file);
```

---

_@MichaReiser reviewed on 2023-08-11 17:31_

---

_Comment by @MichaReiser on 2023-08-11 17:31_

> This PR doesn't implement moving the ellipsis to the same line as the class definition signature. I have never touched the formatter code before, so was looking around about how to do it and found https://github.com/astral-sh/ruff/issues/5822. Seeing as it has its own issue, I might just leave it out of this PR.

That sounds reasonable. We'll need to change all call sites (because they currently always wrap the body in a `block_indent`) and/or introduce a `FormatBody` that we use in all places where a compound statement (not module level) renders a sequence of statements. It can render the ellipsis flat if they have no comments and it is the only statement. 

---

_Label `formatter` added by @MichaReiser on 2023-08-11 17:33_

---

_Review comment by @tjkuson on `crates/ruff_python_formatter/src/statement/suite.rs`:142 on 2023-08-11 18:54_

No, I misunderstood Black's behaviour. I will correct that.

---

_@tjkuson reviewed on 2023-08-11 18:54_

---

_Comment by @tjkuson on 2023-08-11 19:18_

Turns out this doesn't deal with leading comments. For example, it formats to

```python
class Load(expr_context):
    ...
# Some comment.
class Other(expr_context):
    ...
```

I assumed this wouldn't happen because it deals with leading comments in non-stub files. For example,

```python
class Load(expr_context):
    ...


# Some comment.
class Other(expr_context):
    ...
```

---

_Comment by @MichaReiser on 2023-08-14 12:07_

I'll have a closer look at this after merging #6477 and #6452 that both touch the same logic in the suite statement formatting to avoid having to rebase two PRs. I volunteer to rebase your PR. 

---

_Comment by @MichaReiser on 2023-08-15 07:16_

This change improves the typeshed similarity index from 0.74233 to 0.74261



---

_@MichaReiser approved on 2023-08-15 07:16_

---

_Comment by @MichaReiser on 2023-08-15 07:33_

Thank you! Do you want to look into the body formatting of when it is a single `...` next?

---

_Merged by @MichaReiser on 2023-08-15 07:33_

---

_Closed by @MichaReiser on 2023-08-15 07:33_

---

_Branch deleted on 2023-08-15 09:08_

---

_Comment by @tjkuson on 2023-08-15 09:09_

> Thank you! Do you want to look into the body formatting of when it is a single `...` next?

I'll take a look at it.

---
