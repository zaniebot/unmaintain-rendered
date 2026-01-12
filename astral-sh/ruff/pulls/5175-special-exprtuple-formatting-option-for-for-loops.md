```yaml
number: 5175
title: "Special `ExprTuple` formatting option for `for`-loops"
type: pull_request
state: merged
author: konstin
labels:
  - formatter
assignees: []
merged: true
base: main
head: parentheses_unless_breaks
created_at: 2023-06-19T07:31:29Z
updated_at: 2023-06-21T19:35:45Z
url: https://github.com/astral-sh/ruff/pull/5175
synced_at: 2026-01-12T15:55:17Z
```

# Special `ExprTuple` formatting option for `for`-loops

---

_@konstin_

## Motivation

While black keeps parentheses nearly everywhere, the notable exception is in the body of for loops:
```python
for (a, b) in x:
    pass
```
becomes
```python
for a, b in x:
    pass
```

This currently blocks #5163, which this PR should unblock.

## Solution

This changes the `ExprTuple` formatting option to include one additional option that removes the parentheses when not using magic trailing comma and not breaking. It is supposed to be used through
```rust
#[derive(Debug)]
struct ExprTupleWithoutParentheses<'a>(&'a Expr);

impl Format<PyFormatContext<'_>> for ExprTupleWithoutParentheses<'_> {
    fn fmt(&self, f: &mut Formatter<PyFormatContext<'_>>) -> FormatResult<()> {
        match self.0 {
            Expr::Tuple(expr_tuple) => expr_tuple
                .format()
                .with_options(TupleParentheses::StripInsideForLoop)
                .fmt(f),
            other => other.format().with_options(Parenthesize::IfBreaks).fmt(f),
        }
    }
}
```


## Testing

The for loop formatting isn't merged due to missing this (and i didn't want to create more git weirdness across two people), but I've confirmed that when applying this to while loops instead of for loops, then
```rust
        write!(
            f,
            [
                text("while"),
                space(),
                ExprTupleWithoutParentheses(test.as_ref()),
                text(":"),
                trailing_comments(trailing_condition_comments),
                block_indent(&body.format())
            ]
        )?;
```
makes
```python
while (a, b):
    pass

while (
    ajssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssa,
    b,
):
    pass

while (a,b,):
    pass
```
formatted as
```python
while a, b:
    pass

while (
    ajssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssssa,
    b,
):
    pass

while (
    a,
    b,
):
    pass
```

---

_Label `formatter` added by @konstin on 2023-06-19 07:31_

---

_Review requested from @MichaReiser by @konstin on 2023-06-19 07:31_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/parentheses.rs`:79 on 2023-06-19 07:40_

How is this different from `IfBreaks`? `IfBreaks` only adds parentheses if the group breaks but removes them otherwise. 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:179 on 2023-06-19 07:43_

Should the parentheses also be removed if the node has leading comments? 

---

_@MichaReiser reviewed on 2023-06-19 07:43_

---

_Comment by @github-actions[bot] on 2023-06-19 07:43_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.6±0.02ms     6.2 MB/sec    1.00      6.6±0.03ms     6.2 MB/sec
formatter/numpy/ctypeslib.py               1.01   1355.4±2.55µs    12.3 MB/sec    1.00   1348.6±6.33µs    12.3 MB/sec
formatter/numpy/globals.py                 1.00    131.2±0.27µs    22.5 MB/sec    1.00    131.2±3.21µs    22.5 MB/sec
formatter/pydantic/types.py                1.00      2.8±0.01ms     9.3 MB/sec    1.00      2.7±0.00ms     9.3 MB/sec
linter/all-rules/large/dataset.py          1.00     13.3±0.08ms     3.1 MB/sec    1.02     13.5±0.13ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     5.0 MB/sec    1.02      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    429.3±1.22µs     6.9 MB/sec    1.02    436.3±1.68µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.03ms     4.3 MB/sec    1.04      6.1±0.04ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.03ms     6.1 MB/sec    1.01      6.7±0.04ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1449.3±2.36µs    11.5 MB/sec    1.01   1459.3±1.85µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    162.0±0.34µs    18.2 MB/sec    1.01    163.2±0.92µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.01      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.5±0.26ms     4.3 MB/sec    1.05     10.0±0.35ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1990.8±77.98µs     8.4 MB/sec    1.01      2.0±0.10ms     8.3 MB/sec
formatter/numpy/globals.py                 1.02   202.4±12.91µs    14.6 MB/sec    1.00   199.1±11.15µs    14.8 MB/sec
formatter/pydantic/types.py                1.02      4.1±0.12ms     6.2 MB/sec    1.00      4.1±0.11ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     17.7±0.20ms     2.3 MB/sec    1.10     19.5±0.43ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.8±0.07ms     3.5 MB/sec    1.09      5.2±0.16ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   583.5±10.11µs     5.1 MB/sec    1.12   655.9±57.92µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.0±0.14ms     3.2 MB/sec    1.11      8.9±0.24ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00      9.3±0.12ms     4.4 MB/sec    1.07     10.0±0.33ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.03ms     8.3 MB/sec    1.08      2.2±0.06ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   239.9±10.33µs    12.3 MB/sec    1.06   253.1±12.22µs    11.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.6±0.20ms     5.5 MB/sec    1.00      4.6±0.13ms     5.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/parentheses.rs`:79 on 2023-06-19 07:44_

That's different for ExprTuple, because for tuples the parentheses are mandated and are part of the tuple range

---

_@konstin reviewed on 2023-06-19 07:44_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:179 on 2023-06-19 07:45_

in the case where we use this (`for (a, b) in x`) there shouldn't be any leading comments for the tuple expression

---

_@konstin reviewed on 2023-06-19 07:45_

---

_@MichaReiser reviewed on 2023-06-19 08:17_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/parentheses.rs`:79 on 2023-06-19 08:17_

Isn't the same true for `IfStmt` where we remove parentheses in the condition (it uses `IfBreaks`) (and all match cases with an expression)? 

I believe `IfBreaks` already gives you what you need, with the exception that the current `NeedsParentheses` should return `Custom` if `default_needs_parentheses` returns `Parentheses::Optional` instead of `Never` 

---

_@konstin reviewed on 2023-06-19 11:30_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/parentheses.rs`:79 on 2023-06-19 11:30_

If i do that
```
if (a, b):
    pass

(a, b) = x
a, b = x
```
becomes
```
if a, b:
    pass

(a, b) = x
a, b = x
```
which is invalid syntax

---

_@MichaReiser requested changes on 2023-06-19 16:10_

As discussed on discord. The best way is probably to bypass `Expr` and call the Tuple formatting directly from the `for` formatting (and pass a custom option).

---

_Renamed from "Add RemoveUnlessBreaks for removing ExprTuple parentheses" to "Special `ExprTuple` formatting option for `for`-loops" by @konstin on 2023-06-21 18:32_

---

_Review requested from @MichaReiser by @konstin on 2023-06-21 18:34_

---

_Comment by @konstin on 2023-06-21 18:35_

i completely rewrote this

---

_Comment by @davidszotten on 2023-06-21 18:54_

if it makes testing easier, and this is the only blocker, should we consider merging the `StmtFor` pr first? seems ok to merge impls with known issues (not much different from merging and subsequently finding edge cases to fix)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:179 on 2023-06-21 18:58_

Do we still need this? Or is this now covered by the new option?

---

_@MichaReiser reviewed on 2023-06-21 18:58_

---

_Comment by @MichaReiser on 2023-06-21 19:00_

> if it makes testing easier, and this is the only blocker, should we consider merging the `StmtFor` pr first? seems ok to merge impls with known issues (not much different from merging and subsequently finding edge cases to fix)


That sounds reasonable to me. But we can do either 

---

_@konstin reviewed on 2023-06-21 19:01_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:179 on 2023-06-21 19:01_

oh sorry i messed up the git squash, give me a minute

---

_@konstin reviewed on 2023-06-21 19:03_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:179 on 2023-06-21 19:03_

fixed

---

_@MichaReiser approved on 2023-06-21 19:11_

---

_Merged by @konstin on 2023-06-21 19:17_

---

_Closed by @konstin on 2023-06-21 19:17_

---

_Branch deleted on 2023-06-21 19:17_

---

_Comment by @konstin on 2023-06-21 19:19_

Either works fine, but i think it's easier to test if merge or rebase main into https://github.com/astral-sh/ruff/pull/5163

---
