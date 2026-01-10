```yaml
number: 21720
title: "parser: parsing doublestar as a power op without a left hand side"
type: pull_request
state: open
author: 11happy
labels:
  - parser
assignees: []
base: main
head: double_star_as_pow
created_at: 2025-12-01T09:13:26Z
updated_at: 2025-12-15T07:58:31Z
url: https://github.com/astral-sh/ruff/pull/21720
synced_at: 2026-01-10T16:42:11Z
```

# parser: parsing doublestar as a power op without a left hand side

---

_Pull request opened by @11happy on 2025-12-01 09:13_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Parses `**` Doublestar as power op without lhs, Part of #10653, referred discussion https://github.com/astral-sh/ruff/pull/10636#discussion_r1542405830

## Test Plan

<!-- How was it tested? -->
test case :
```python
{x: **y for x, y in data}
```

ruff-dev output before:
```
Error: Expected an expression at byte range 4..6

Caused by:
    Expected an expression
```

ruff-dev output ast after:
```
Module(
    ModModule {
        node_index: NodeIndex(None),
        range: 0..25,
        body: [
            Expr(
                StmtExpr {
                    node_index: NodeIndex(None),
                    range: 0..25,
                    value: DictComp(
                        ExprDictComp {
                            node_index: NodeIndex(None),
                            range: 0..25,
                            key: Name(
                                ExprName {
                                    node_index: NodeIndex(None),
                                    range: 1..2,
                                    id: Name("x"),
                                    ctx: Load,
                                },
                            ),
                            value: BinOp(
                                ExprBinOp {
                                    node_index: NodeIndex(None),
                                    range: 4..7,
                                    left: Name(
                                        ExprName {
                                            node_index: NodeIndex(None),
                                            range: 7..7,
                                            id: Name(""),
                                            ctx: Invalid,
                                        },
                                    ),
                                    op: Pow,
                                    right: Name(
                                        ExprName {
                                            node_index: NodeIndex(None),
                                            range: 6..7,
                                            id: Name("y"),
                                            ctx: Load,
                                        },
                                    ),
                                },
                            ),
                            generators: [
                                Comprehension {
                                    range: 8..24,
                                    node_index: NodeIndex(None),
                                    target: Tuple(
                                        ExprTuple {
                                            node_index: NodeIndex(None),
                                            range: 12..16,
                                            elts: [
                                                Name(
                                                    ExprName {
                                                        node_index: NodeIndex(None),
                                                        range: 12..13,
                                                        id: Name("x"),
                                                        ctx: Store,
                                                    },
                                                ),
                                                Name(
                                                    ExprName {
                                                        node_index: NodeIndex(None),
                                                        range: 15..16,
                                                        id: Name("y"),
                                                        ctx: Store,
                                                    },
                                                ),
                                            ],
                                            ctx: Store,
                                            parenthesized: false,
                                        },
                                    ),
                                    iter: Name(
                                        ExprName {
                                            node_index: NodeIndex(None),
                                            range: 20..24,
                                            id: Name("data"),
                                            ctx: Load,
                                        },
                                    ),
                                    ifs: [],
                                    is_async: false,
                                },
                            ],
                        },
                    ),
                },
            ),
        ],
    },
)


```


---

_Review requested from @MichaReiser by @11happy on 2025-12-01 09:13_

---

_Review requested from @dhruvmanila by @11happy on 2025-12-01 09:13_

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 09:31_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_@MichaReiser requested changes on 2025-12-01 10:03_

Would you mind adding a few parser tests that demonstrate that the new recovery logic works as expected? 

E.g., the ecosystem changes suggest that we:

a) Fail to emit a diagnostic in that case
b) or we now use this code path for `**kwargs` when we shouldn't

---

_Label `parser` added by @MichaReiser on 2025-12-01 10:03_

---

_Comment by @11happy on 2025-12-02 07:30_

 > Would you mind adding a few parser tests that demonstrate that the new recovery logic works as expected?

 > E.g., the ecosystem changes suggest that we:

 > a) Fail to emit a diagnostic in that case
 > b) or we now use this code path for **kwargs when we shouldn't

I have added tests & I think its not the option b, I would appreciate any pointers to move forward

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:382 on 2025-12-05 08:18_

I'm not sure if this is the right place for the fix. Would it be possible to add it to `parse_binary_expression_or_higher` or even in `parse_binary_expression_or_higher_recursive`? If not, can you explain why?

Moving it their would give you the diagnostic for free

---

_@MichaReiser reviewed on 2025-12-05 08:19_

Would you mind changing your PR to use the testing infrastructure described here

https://github.com/astral-sh/ruff/blob/f7bd94bd27eac49db9efa7fbd0b598a3c0cde806/crates/ruff_python_parser/CONTRIBUTING.md?plain=1#L5


Can you also add an example where there's some content to the right, e.g. `** a + 3 * 2`

---

_@11happy reviewed on 2025-12-08 12:54_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/parser/expression.rs`:382 on 2025-12-08 12:54_

I think this might be correct place as we need to detect the missing left operand & in `parse_binary_expression_or_higher_recursive` the LHS is passed already as a `ParsedExpr` 

---

_@11happy reviewed on 2025-12-08 12:56_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/parser/expression.rs`:382 on 2025-12-08 12:56_

done I have written the inline tests.

---

_Comment by @11happy on 2025-12-14 08:27_

ping! 
would appreciate a review : )

---

_@MichaReiser reviewed on 2025-12-14 09:28_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:371 on 2025-12-14 09:28_

This should be an invalid test and the parser should emit a diagnostic

---

_@11happy reviewed on 2025-12-15 07:26_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/parser/expression.rs`:371 on 2025-12-15 07:26_

done 
Thank you : )

---
