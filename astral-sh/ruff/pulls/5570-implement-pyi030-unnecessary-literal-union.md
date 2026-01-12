```yaml
number: 5570
title: "Implement PYI030: Unnecessary literal union"
type: pull_request
state: merged
author: zanieb
labels:
  - rule
assignees: []
merged: true
base: main
head: rule/pyi030
created_at: 2023-07-06T19:00:46Z
updated_at: 2023-07-07T17:05:56Z
url: https://github.com/astral-sh/ruff/pull/5570
synced_at: 2026-01-12T03:36:55Z
```

# Implement PYI030: Unnecessary literal union

---

_Pull request opened by @zanieb on 2023-07-06 19:00_

Implements PYI030 as part of https://github.com/astral-sh/ruff/issues/848

> Union expressions should never have more than one Literal member, as Literal[1] | Literal[2] is semantically identical to Literal[1, 2].

Note we differ slightly from the flake8-pyi implementation:

- We detect cases where there are parentheses or nested unions
- We detect cases with mixed `Union` and `|` syntax
- We use the same error message for all violations; flake8-pyi has two different messages
- We retain the user's quoting style when displaying string literals; flake8-pyi uses single quotes
- We warn on duplicates of the same literal `Literal[1] | Literal[1]`
    - flake8-pyi does not warn on this because it overlaps with PYI016; we can avoid a violation here if we want but then we'll need to track the hashes of all seen literals alongside the vector of literal expressions which I would prefer to keep ordered

```
❯ flake8 --select Y030 crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:9:9: Y030 Multiple Literal members in a union. Use a single Literal, e.g. "Literal[1, 2]".
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:12:17: Y030 Multiple Literal members in a union. Use a single Literal, e.g. "Literal[1, 2]".
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:17:16: Y030 Multiple Literal members in a union. Use a single Literal, e.g. "Literal[1, 2]".
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:22:9: Y030 Multiple Literal members in a union. Combine them into one, e.g. "Literal[1, 2]".
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:23:9: Y030 Multiple Literal members in a union. Combine them into one, e.g. "Literal[1, 2]".
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:24:9: Y030 Multiple Literal members in a union. Combine them into one, e.g. "Literal[1, 2]".
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:25:9: Y030 Multiple Literal members in a union. Combine them into one, e.g. "Literal[1, 2]".
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:28:10: Y030 Multiple Literal members in a union. Use a single Literal, e.g. "Literal[1, 2]".
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:35:11: Y030 Multiple Literal members in a union. Combine them into one, e.g. "Literal[1, 2]".
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:38:15: Y030 Multiple Literal members in a union. Use a single Literal, e.g. "Literal[1, 2]".
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:41:10: Y030 Multiple Literal members in a union. Use a single Literal, e.g. "Literal[1, 2, 3]".
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:42:10: Y030 Multiple Literal members in a union. Use a single Literal, e.g. "Literal[1, 2, 3, 4]".
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:45:10: Y030 Multiple Literal members in a union. Combine them into one, e.g. "Literal[1, 2, 3]".
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:48:10: Y030 Multiple Literal members in a union. Use a single Literal, e.g. "Literal[1, 'foo', True]".
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:60:10: Y030 Multiple Literal members in a union. Use a single Literal, e.g. "Literal[1, 2]".
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:64:5: Y030 Multiple Literal members in a union. Use a single Literal, e.g. "Literal[1, 2]".
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:71:10: Y030 Multiple Literal members in a union. Use a single Literal, e.g. "Literal[1, 2, 3, 4]".
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:74:23: Y030 Multiple Literal members in a union. Use a single Literal, e.g. "Literal[1, 2]".
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:77:10: Y030 Multiple Literal members in a union. Use a single Literal, e.g. "Literal[1, 2]".
(19 errors)
```

```
❯ ./target/debug/ruff --select PYI030 crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:9:9: PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal[1, 2]`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:12:17: PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal[1, 2]`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:17:16: PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal[1, 2]`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:22:9: PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal[1, 2]`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:23:9: PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal[1, 2]`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:24:9: PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal[1, 2]`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:25:9: PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal[1, 2]`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:28:10: PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal[1, 2]`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:31:9: PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal[1, 2]`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:34:9: PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal[1, 2]`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:35:10: PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal[1, 2]`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:38:15: PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal[1, 2]`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:41:10: PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal[1, 2, 3]`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:42:10: PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal[1, 2, 3, 4]`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:45:10: PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal[1, 2, 3]`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:48:10: PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal[1, "foo", True]`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:51:10: PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal[1, 1]`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:60:10: PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal[1, 2]`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:63:10: PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal[1, 2]`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:71:10: PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal[1, 2, 3, 4]`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:74:10: PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal[1, 2]`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:77:10: PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal[1, 2]`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:80:10: PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal[1, 2]`
crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi:83:10: PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal[1, 2]`
Found 24 errors.
```

---

_Renamed from "Implement PYI0303: Unnecessary literal union" to "Implement PYI030: Unnecessary literal union" by @zanieb on 2023-07-06 19:01_

---

_Comment by @github-actions[bot] on 2023-07-06 19:13_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+3, -0, 0 error(s))

<details><summary>airflow (+2, -0)</summary>
<p>

```diff
+ airflow/cli/commands/task_command.py:72:21: PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal[False, "db", "memory"]`
+ airflow/models/mappedoperator.py:80:20: PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal["expand", "partial"]`
```

</p>
</details>
<details><summary>bokeh (+1, -0)</summary>
<p>

```diff
+ src/bokeh/core/serialization.py:148:25: PYI030 Multiple literal members in a union. Use a single literal, e.g. `Literal["bool", "object"]`
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PYI030 | 3 | 3 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.1±0.37ms     4.5 MB/sec    1.00      9.0±0.45ms     4.5 MB/sec
formatter/numpy/ctypeslib.py               1.00  1960.9±78.37µs     8.5 MB/sec    1.02  1996.2±111.16µs     8.3 MB/sec
formatter/numpy/globals.py                 1.00   228.9±15.92µs    12.9 MB/sec    1.02   232.9±20.43µs    12.7 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.22ms     5.9 MB/sec    1.02      4.4±0.25ms     5.8 MB/sec
linter/all-rules/large/dataset.py          1.02     15.9±0.55ms     2.6 MB/sec    1.00     15.6±0.50ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.0±0.26ms     4.2 MB/sec    1.00      3.9±0.20ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.01   507.1±34.80µs     5.8 MB/sec    1.00   500.2±27.49µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.03      7.0±0.34ms     3.6 MB/sec    1.00      6.8±0.28ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.6±0.28ms     5.4 MB/sec    1.00      7.6±0.23ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1619.5±67.59µs    10.3 MB/sec    1.02  1645.2±78.43µs    10.1 MB/sec
linter/default-rules/numpy/globals.py      1.00   193.8±12.17µs    15.2 MB/sec    1.03   200.3±14.57µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.14ms     7.5 MB/sec    1.04      3.5±0.18ms     7.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.9±0.38ms     3.4 MB/sec    1.00     11.9±0.48ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.6±0.12ms     6.4 MB/sec    1.01      2.6±0.10ms     6.4 MB/sec
formatter/numpy/globals.py                 1.00   293.6±19.61µs    10.1 MB/sec    1.01   296.5±18.84µs    10.0 MB/sec
formatter/pydantic/types.py                1.00      5.5±0.26ms     4.6 MB/sec    1.04      5.8±0.28ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.01     20.5±0.66ms  2031.6 KB/sec    1.00     20.2±0.55ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.4±0.29ms     3.1 MB/sec    1.00      5.3±0.18ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   654.2±44.01µs     4.5 MB/sec    1.01   663.5±36.00µs     4.4 MB/sec
linter/all-rules/pydantic/types.py         1.03      9.2±0.39ms     2.8 MB/sec    1.00      8.9±0.31ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.01     10.4±0.50ms     3.9 MB/sec    1.00     10.3±0.30ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.11ms     7.6 MB/sec    1.03      2.3±0.09ms     7.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   253.7±17.24µs    11.6 MB/sec    1.03   261.8±15.93µs    11.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.21ms     5.6 MB/sec    1.02      4.6±0.17ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/unnecessary_literal_union.rs`:12 on 2023-07-06 19:40_

Annoyingly, we try to wrap these at 80 characters so that they stick to that maximum width when rendered in the terminal :(

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi`:73 on 2023-07-06 19:41_

Nit: extra backtick here.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/unnecessary_literal_union.rs`:62 on 2023-07-06 19:43_

Nit: consider renaming `m` to `member` here for explicitness? We tend to avoid single-variable names outside of (e.g.) indexes, but it's obviously not a blocker, up to your discretion.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/snapshots/ruff__rules__flake8_pyi__tests__PYI030_PYI030.pyi.snap`:171 on 2023-07-06 19:45_

Notice here how we're flagging line 42 three times. I think we need to apply the same guards that we apply to the `duplicate_union_member` call in `ast/mod.rs`: if the parent of the current expression is an `|`, avoid running the rule, since we'll already have analyzed the current expression.

---

_@charliermarsh reviewed on 2023-07-06 19:45_

Looking good!

---

_@zanieb reviewed on 2023-07-06 21:06_

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_pyi/snapshots/ruff__rules__flake8_pyi__tests__PYI030_PYI030.pyi.snap`:171 on 2023-07-06 21:06_

I've just noticed this and was about to do some investigation.

I'm guessing that's the purpose of this uncommented code for the similar rule?

https://github.com/charliermarsh/ruff/blob/9c7194aa83dd8cf6e6dbb59c89f4524f063ba848/crates/ruff/src/checkers/ast/mod.rs#L3144-L3152

---

_@charliermarsh reviewed on 2023-07-06 21:09_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/snapshots/ruff__rules__flake8_pyi__tests__PYI030_PYI030.pyi.snap`:171 on 2023-07-06 21:09_

Yup exactly -- it checks if we're in a child of one of the expressions we _presumedly_ already visited.

---

_@zanieb reviewed on 2023-07-06 21:37_

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_pyi/rules/unnecessary_literal_union.rs`:78 on 2023-07-06 21:37_

In a follow-up, we can probably use this in PYI016 (https://github.com/astral-sh/ruff/pull/3922) and fix support for `typing.Union` there

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_pyi/snapshots/ruff__rules__flake8_pyi__tests__PYI030_PYI030.pyi.snap`:154 on 2023-07-06 21:38_

We should probably avoid emitting this but I'm hesitant to add a hash map alongside the vector used to display items in the correct order.

---

_@zanieb reviewed on 2023-07-06 21:38_

---

_Comment by @zanieb on 2023-07-06 21:43_

Airflow example https://github.com/apache/airflow/blob/358e6e8fa18166084fc17b23e75c6c29a37f245f/airflow/cli/commands/task_command.py#L72

Bokeh example https://github.com/bokeh/bokeh/blob/ee3f7f27fb744860763edd1d44fcac9b797e36b4/src/bokeh/core/serialization.py#L148



---

_@zanieb reviewed on 2023-07-06 22:22_

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_pyi/snapshots/ruff__rules__flake8_pyi__tests__PYI030_PYI030.pyi.snap`:171 on 2023-07-06 22:22_

Needed https://github.com/astral-sh/ruff/pull/5570/commits/1759d914456b62dfc479518ce8717949935f6c87 as well

---

_Marked ready for review by @zanieb on 2023-07-06 22:22_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:3154 on 2023-07-06 22:27_

We may want to add these (`&& self.semantic.in_type_definition()`) to the `Rule::UnnecessaryLiteralUnion` condition too. I can't quite remember why they're necessary here. I'm guessing it's because for `|`, if it's used outside of a type definition, it's not a union operator (it's a bitwise or, I think).

---

_@charliermarsh reviewed on 2023-07-06 22:27_

---

_@charliermarsh reviewed on 2023-07-06 22:27_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/unnecessary_literal_union.rs`:21 on 2023-07-06 22:27_

Nit: maybe `from typing import Literal` on these for completeness in the examples?

---

_Comment by @zanieb on 2023-07-06 22:28_

Note we could implement auto-fix for this but it seems painful given the enumerated edge-cases in the test file. We can pursue it at a later time if requested.

---

_@charliermarsh approved on 2023-07-06 22:29_

LGTM! Couple small comments, address at your discretion.

---

_Review comment by @zanieb on `crates/ruff/src/checkers/ast/mod.rs`:3154 on 2023-07-06 22:31_

That's true for the `DuplicateUnionMember` rule where they're checking for _any_ duplicate items in a `|` but here we know we are constrained to literal types where this does apply.

We test for this at 

https://github.com/charliermarsh/ruff/blob/1759d914456b62dfc479518ce8717949935f6c87/crates/ruff/resources/test/fixtures/flake8_pyi/PYI030.pyi#L28

You can see that a bitwise or of literals resolve to a `Union` outside of an annotation:

```
In [3]: Literal[1] | Literal[2]
Out[3]: typing.Union[typing.Literal[1], typing.Literal[2]]

In [4]: x = Literal[1] | Literal[2]

In [5]: x
Out[5]: typing.Union[typing.Literal[1], typing.Literal[2]]
```

---

_@zanieb reviewed on 2023-07-06 22:31_

---

_@zanieb reviewed on 2023-07-06 22:33_

---

_Review comment by @zanieb on `crates/ruff/src/checkers/ast/mod.rs`:3154 on 2023-07-06 22:33_

I guess it's possible that someone has `Literal[1] | bar | Literal[2]` where `bar` implements some sort of custom bitwise or behavior and this rule no longer holds but I'm not sure that's worth handling

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:3154 on 2023-07-06 22:34_

Interesting, that makes sense. Thanks for the thorough follow-up!

---

_@charliermarsh reviewed on 2023-07-06 22:34_

---

_Label `rule` added by @charliermarsh on 2023-07-06 22:36_

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:2201 on 2023-07-07 09:32_

Nit: I think this may work
```suggestion
                    if let Some(Expr::Subscript(ast::ExprSubscript { value, .. })) = self.semantic.expr_parent()
                         !self.semantic.match_typing_expr(value, "Union")
                    } else {
                   	false
                    } {
```

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:3177 on 2023-07-07 09:33_

I think you can flatten this by using the `matches` directly on the `expr_parent`

```suggestion
                        && matches!(self.semantic.expr_parent(), Some(Expr::BinOp(ast::ExprBinOp { op: Operator::BitOr,                                    .. })
```

... I've probably messed up the parentheses but you get the idea ;)

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_pyi/rules/unnecessary_literal_union.rs`:46 on 2023-07-07 09:36_

We may want to consider using a `SmallVec` e.g. with a size of `1` to avoid allocating a `Vec` on the heap for all *proper* literal union usages

https://docs.rs/smallvec/latest/smallvec/struct.SmallVec.html

---

_@MichaReiser approved on 2023-07-07 09:40_

---

_Merged by @zanieb on 2023-07-07 16:43_

---

_Closed by @zanieb on 2023-07-07 16:43_

---
