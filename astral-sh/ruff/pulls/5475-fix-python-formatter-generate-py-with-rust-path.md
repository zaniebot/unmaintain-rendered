```yaml
number: 5475
title: Fix python_formatter generate.py with rust path
type: pull_request
state: merged
author: LouisDISPA
labels: []
assignees: []
merged: true
base: main
head: main
created_at: 2023-07-03T12:42:18Z
updated_at: 2023-07-03T14:09:40Z
url: https://github.com/astral-sh/ruff/pull/5475
synced_at: 2026-01-12T15:55:18Z
```

# Fix python_formatter generate.py with rust path

---

_@LouisDISPA_

## Summary

This PR fix an issue with the `generate.py` file of the python formatter.  
Since https://github.com/astral-sh/ruff/pull/5369 the [node.rs file](https://github.com/astral-sh/ruff/blob/f51dc204979964b7a7d9a2469c0c1b0c06d26980/crates/ruff_python_ast/src/node.rs) used to generate the types now has `ast::` in the enum.

```rust
pub enum AnyNode {
   ModModule(ModModule),
   ModInteractive(ModInteractive),
   ModExpression(ModExpression),
   ModFunctionType(ModFunctionType),
   ...
```

And now:

```rust
pub enum AnyNode {
   ModModule(ast::ModModule),
   ModInteractive(ast::ModInteractive),
   ModExpression(ast::ModExpression),
   ModFunctionType(ast::ModFunctionType),
   ...
```

The python script was not parsing rust paths. This PR adds the possibility to have it.

## Test Plan

This was tested locally.

### Script output

Before

```
['ast::ModModule),', 'ast::ModInteractive),', 'ast::ModExpression),', 'ast::ModFunctionType),', 'ast::StmtFunctionDef),', 'ast::StmtAsyncFunctionDef),', 'ast::StmtClassDef),', 'ast::StmtReturn),', 'ast::StmtDelete),', 'ast::StmtAssign),', 'ast::StmtAugAssign),', 'ast::StmtAnnAssign),', 'ast::StmtFor),', 'ast::StmtAsyncFor),', 'ast::StmtWhile),', 'ast::StmtIf),', 'ast::StmtWith),', 'ast::StmtAsyncWith),', 'ast::StmtMatch),', 'ast::StmtRaise),', 'ast::StmtTry),', 'ast::StmtTryStar),', 'ast::StmtAssert),', 'ast::StmtImport),', 'ast::StmtImportFrom),', 'ast::StmtGlobal),', 'ast::StmtNonlocal),', 'ast::StmtExpr),', 'ast::StmtPass),', 'ast::StmtBreak),', 'ast::StmtContinue),', 'ast::ExprBoolOp),', 'ast::ExprNamedExpr),', 'ast::ExprBinOp),', 'ast::ExprUnaryOp),', 'ast::ExprLambda),', 'ast::ExprIfExp),', 'ast::ExprDict),', 'ast::ExprSet),', 'ast::ExprListComp),', 'ast::ExprSetComp),', 'ast::ExprDictComp),', 'ast::ExprGeneratorExp),', 'ast::ExprAwait),', 'ast::ExprYield),', 'ast::ExprYieldFrom),', 'ast::ExprCompare),', 'ast::ExprCall),', 'ast::ExprFormattedValue),', 'ast::ExprJoinedStr),', 'ast::ExprConstant),', 'ast::ExprAttribute),', 'ast::ExprSubscript),', 'ast::ExprStarred),', 'ast::ExprName),', 'ast::ExprList),', 'ast::ExprTuple),', 'ast::ExprSlice),', 'ast::ExceptHandlerExceptHandler),', 'ast::PatternMatchValue),', 'ast::PatternMatchSingleton),', 'ast::PatternMatchSequence),', 'ast::PatternMatchMapping),', 'ast::PatternMatchClass),', 'ast::PatternMatchStar),', 'ast::PatternMatchAs),', 'ast::PatternMatchOr),', 'ast::TypeIgnoreTypeIgnore),', 'Comprehension),', 'Arguments),', 'Arg),', 'ArgWithDefault),', 'Keyword),', 'Alias),', 'WithItem),', 'MatchCase),', 'Decorator),']

error: unexpected closing delimiter: `)`
 --> <stdin>:3:55
  |
2 |             use ruff_formatter::{write, Buffer, FormatResult};
  |                                 - this opening brace...     - ...matches this closing brace
3 |             use rustpython_parser::ast::ast::ModModule),;
  |                                                       ^ unexpected closing delimiter

Traceback (most recent call last):
  File "/Users/ldispa/Documents/perso/ruff/crates/ruff_python_formatter/generate.py", line 100, in <module>
    node_path.write_text(rustfmt(code))
                         ^^^^^^^^^^^^^
  File "/Users/ldispa/Documents/perso/ruff/crates/ruff_python_formatter/generate.py", line 12, in rustfmt
    return check_output(["rustfmt", "--emit=stdout"], input=code, text=True)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/opt/homebrew/Cellar/python@3.11/3.11.4_1/Frameworks/Python.framework/Versions/3.11/lib/python3.11/subprocess.py", line 466, in check_output
    return run(*popenargs, stdout=PIPE, timeout=timeout, check=True,
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/opt/homebrew/Cellar/python@3.11/3.11.4_1/Frameworks/Python.framework/Versions/3.11/lib/python3.11/subprocess.py", line 571, in run
    raise CalledProcessError(retcode, process.args,
subprocess.CalledProcessError: Command '['rustfmt', '--emit=stdout']' returned non-zero exit status 1.
```

After:
```
['ModModule', 'ModInteractive', 'ModExpression', 'ModFunctionType', 'StmtFunctionDef', 'StmtAsyncFunctionDef', 'StmtClassDef', 'StmtReturn', 'StmtDelete', 'StmtAssign', 'StmtAugAssign', 'StmtAnnAssign', 'StmtFor', 'StmtAsyncFor', 'StmtWhile', 'StmtIf', 'StmtWith', 'StmtAsyncWith', 'StmtMatch', 'StmtRaise', 'StmtTry', 'StmtTryStar', 'StmtAssert', 'StmtImport', 'StmtImportFrom', 'StmtGlobal', 'StmtNonlocal', 'StmtExpr', 'StmtPass', 'StmtBreak', 'StmtContinue', 'ExprBoolOp', 'ExprNamedExpr', 'ExprBinOp', 'ExprUnaryOp', 'ExprLambda', 'ExprIfExp', 'ExprDict', 'ExprSet', 'ExprListComp', 'ExprSetComp', 'ExprDictComp', 'ExprGeneratorExp', 'ExprAwait', 'ExprYield', 'ExprYieldFrom', 'ExprCompare', 'ExprCall', 'ExprFormattedValue', 'ExprJoinedStr', 'ExprConstant', 'ExprAttribute', 'ExprSubscript', 'ExprStarred', 'ExprName', 'ExprList', 'ExprTuple', 'ExprSlice', 'ExceptHandlerExceptHandler', 'PatternMatchValue', 'PatternMatchSingleton', 'PatternMatchSequence', 'PatternMatchMapping', 'PatternMatchClass', 'PatternMatchStar', 'PatternMatchAs', 'PatternMatchOr', 'TypeIgnoreTypeIgnore', 'Comprehension', 'Arguments', 'Arg', 'ArgWithDefault', 'Keyword', 'Alias', 'WithItem', 'MatchCase', 'Decorator']
```

---

_Comment by @github-actions[bot] on 2023-07-03 12:55_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.6±0.29ms     4.2 MB/sec    1.00      9.5±0.28ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.1±0.09ms     7.9 MB/sec    1.00      2.1±0.10ms     7.9 MB/sec
formatter/numpy/globals.py                 1.00   237.9±10.83µs    12.4 MB/sec    1.02   243.8±10.34µs    12.1 MB/sec
formatter/pydantic/types.py                1.00      4.5±0.12ms     5.6 MB/sec    1.01      4.6±0.16ms     5.6 MB/sec
linter/all-rules/large/dataset.py          1.00     17.1±0.45ms     2.4 MB/sec    1.01     17.3±0.33ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.10ms     4.0 MB/sec    1.00      4.1±0.10ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   545.3±30.02µs     5.4 MB/sec    1.00   543.5±12.82µs     5.4 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.5±0.45ms     3.4 MB/sec    1.00      7.4±0.13ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.23ms     5.0 MB/sec    1.00      8.2±0.14ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1787.2±68.91µs     9.3 MB/sec    1.01  1799.1±100.47µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    208.1±5.78µs    14.2 MB/sec    1.01   210.6±14.04µs    14.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.09ms     6.9 MB/sec    1.00      3.7±0.06ms     6.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      9.6±0.16ms     4.2 MB/sec    1.00      9.4±0.12ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.04ms     8.2 MB/sec    1.00      2.0±0.04ms     8.2 MB/sec
formatter/numpy/globals.py                 1.00    227.2±4.36µs    13.0 MB/sec    1.02   231.6±16.16µs    12.7 MB/sec
formatter/pydantic/types.py                1.00      4.4±0.06ms     5.7 MB/sec    1.00      4.5±0.06ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     15.9±0.24ms     2.6 MB/sec    1.04     16.4±0.36ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.04ms     4.1 MB/sec    1.04      4.3±0.09ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    503.7±8.12µs     5.9 MB/sec    1.00    501.8±5.32µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.09ms     3.6 MB/sec    1.01      7.1±0.11ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.02      8.4±0.14ms     4.9 MB/sec    1.00      8.2±0.09ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1796.0±51.13µs     9.3 MB/sec    1.00  1749.6±15.79µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.01    206.2±4.88µs    14.3 MB/sec    1.00    204.1±3.92µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.8±0.07ms     6.7 MB/sec    1.00      3.7±0.03ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin approved on 2023-07-03 14:07_

---

_Merged by @konstin on 2023-07-03 14:07_

---

_Closed by @konstin on 2023-07-03 14:07_

---

_Comment by @charliermarsh on 2023-07-03 14:09_

Thanks!

---
