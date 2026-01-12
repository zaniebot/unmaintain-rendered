```yaml
number: 10523
title: "Avoid dropping node from `as` pattern"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: dhruv/pattern-recovery
head: dhruv/pattern-recovery-3
created_at: 2024-03-22T10:00:17Z
updated_at: 2024-03-22T14:53:45Z
url: https://github.com/astral-sh/ruff/pull/10523
synced_at: 2026-01-12T15:55:32Z
```

# Avoid dropping node from `as` pattern

---

_@dhruvmanila_

## Summary

This PR addresses the feedback given here: https://github.com/astral-sh/ruff/pull/10477#discussion_r1533540673

**Problem:** The `as` pattern containing both the pattern and name (`foo as bar`) cannot be converted to an expression as there's no AST node to represent that.

The solution for now is to not drop any nodes, panic in the conversion if the parser is doing so, and make sure the calling code is avoiding the panic.

There are 3 places where the conversion happens:
1. Mapping pattern
2. Class pattern
3. Complex literal pattern

Another thing to keep in mind is that the `parse_match_pattern_lhs` method only parses the `as` pattern if it's parenthesized. The reason being that the `(` is ambiguous as to whether it's a parenthesized pattern or the start of a sequence pattern.

Now, the mapping items are parsed using list parsing and it's only called if the start token is in the FIRST set which the `(` token isn't. This avoids the panic in mapping pattern.

For the class and complex literal pattern, the starting point is `parse_match_pattern_lhs` method. It then checks if the parser is at `(` (as in `Foo(x, y)`) or `+`/`-` (as in `1 + 1j`) and continues with the respective parsing methods. We want to avoid going further in case we have an `as` pattern to avoid the panic. This means we'll exit early in that case.

## Test Plan

Added a few test cases, updated the snapshots.

Note that this PR isn't focused on error recovery, it's to avoid dropping any potential node which is the case when converting from an `as` pattern to an expression.

Another thing to note is that the following test case fails (not added) because the error recovery is bad and the node ranges aren't in order:

```python
match subject:
    case {x as y: 1}:
        pass
```

<details><summary>Click here to view the AST and errors for the above code:</summary>
<p>

```rs
Module(
    ModModule {
        range: 0..50,
        body: [
            Match(
                StmtMatch {
                    range: 0..49,
                    subject: Name(
                        ExprName {
                            range: 6..13,
                            id: "subject",
                            ctx: Load,
                        },
                    ),
                    cases: [
                        MatchCase {
                            range: 19..49,
                            pattern: MatchMapping(
                                PatternMatchMapping {
                                    range: 24..35,
                                    keys: [
                                        Name(
                                            ExprName {
                                                range: 25..26,
                                                id: "x",
                                                ctx: Store,
                                            },
                                        ),
                                        Name(
                                            ExprName {
                                                range: 30..31,
                                                id: "y",
                                                ctx: Store,
                                            },
                                        ),
                                    ],
                                    patterns: [
                                        MatchValue(
                                            PatternMatchValue {
                                                range: 29..29,
                                                value: Name(
                                                    ExprName {
                                                        range: 27..29,
                                                        id: "as",
                                                        ctx: Load,
                                                    },
                                                ),
                                            },
                                        ),
                                        MatchValue(
                                            PatternMatchValue {
                                                range: 33..34,
                                                value: NumberLiteral(
                                                    ExprNumberLiteral {
                                                        range: 33..34,
                                                        value: Int(
                                                            1,
                                                        ),
                                                    },
                                                ),
                                            },
                                        ),
                                    ],
                                    rest: None,
                                },
                            ),
                            guard: None,
                            body: [
                                Pass(
                                    StmtPass {
                                        range: 45..49,
                                    },
                                ),
                            ],
                        },
                    ],
                },
            ),
        ],
    },
)
```

Errors:
```
Syntax Error: invalid mapping pattern key at byte range 25..26
  |
1 | match subject:
2 |     case {x as y: 1}:
  |           ^ Syntax Error: invalid mapping pattern key
3 |         pass
  |


Syntax Error: expected Colon, found As at byte range 27..29
  |
1 | match subject:
2 |     case {x as y: 1}:
  |             ^^ Syntax Error: expected Colon, found As
3 |         pass
  |


Syntax Error: expected Comma, found Name at byte range 30..31
  |
1 | match subject:
2 |     case {x as y: 1}:
  |                ^ Syntax Error: expected Comma, found Name
3 |         pass
  |
```

</p>
</details> 


---

_Label `parser` added by @dhruvmanila on 2024-03-22 10:00_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-22 10:00_

---

_Comment by @github-actions[bot] on 2024-03-22 10:20_

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

_@MichaReiser approved on 2024-03-22 13:16_

Do you plan to look into the incorrect range issue of the following code (that you shared) separately? 

```
match subject:
    case {x as y: 1}:
        pass
``` 

---

_Comment by @dhruvmanila on 2024-03-22 14:53_

> Do you plan to look into the incorrect range issue of the following code (that you shared) separately?

Yeah, my hunch is that it's got to do something with the empty nodes.

---

_Merged by @dhruvmanila on 2024-03-22 14:53_

---

_Closed by @dhruvmanila on 2024-03-22 14:53_

---

_Branch deleted on 2024-03-22 14:53_

---
