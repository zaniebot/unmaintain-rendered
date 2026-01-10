```yaml
number: 2365
title: "Completions for `[item.<CURSOR> for item in xs]` fail because of how the comprehension is parsed"
type: issue
state: open
author: BurntSushi
labels:
  - completions
assignees: []
created_at: 2026-01-06T12:38:58Z
updated_at: 2026-01-06T13:06:46Z
url: https://github.com/astral-sh/ty/issues/2365
synced_at: 2026-01-10T01:56:41Z
```

# Completions for `[item.<CURSOR> for item in xs]` fail because of how the comprehension is parsed

---

_Issue opened by @BurntSushi on 2026-01-06 12:38_

### Summary

This video shows how the type of `item` in `[item.<CURSOR> for item in xs]` is not known, and that in turn is the cause for completions not working:

https://github.com/user-attachments/assets/0d0b1ab5-c2a7-4e80-b639-0dd358857c68

The reason for why the type of `item` is not known seems likely related to how this [invalid comprehension is parsed](https://play.ty.dev/c6d74417-84cd-482f-80fe-95716636b902):

```
ModModule {
    node_index: NodeIndex(None),
    range: 0..43,
    body: [
        AnnAssign(
            StmtAnnAssign {
                node_index: NodeIndex(
                    1,
                ),
                range: 0..20,
                target: Name(
                    ExprName {
                        node_index: NodeIndex(
                            2,
                        ),
                        range: 0..2,
                        id: Name("xs"),
                        ctx: Store,
                    },
                ),
                annotation: Subscript(
                    ExprSubscript {
                        node_index: NodeIndex(
                            4,
                        ),
                        range: 4..13,
                        value: Name(
                            ExprName {
                                node_index: NodeIndex(
                                    5,
                                ),
                                range: 4..8,
                                id: Name("list"),
                                ctx: Load,
                            },
                        ),
                        slice: Name(
                            ExprName {
                                node_index: NodeIndex(
                                    6,
                                ),
                                range: 9..12,
                                id: Name("str"),
                                ctx: Load,
                            },
                        ),
                        ctx: Load,
                    },
                ),
                value: Some(
                    List(
                        ExprList {
                            node_index: NodeIndex(
                                7,
                            ),
                            range: 16..20,
                            elts: [
                                StringLiteral(
                                    ExprStringLiteral {
                                        node_index: NodeIndex(
                                            8,
                                        ),
                                        range: 17..19,
                                        value: StringLiteralValue {
                                            inner: Single(
                                                StringLiteral {
                                                    range: 17..19,
                                                    node_index: NodeIndex(
                                                        9,
                                                    ),
                                                    value: "",
                                                    flags: StringLiteralFlags {
                                                        quote_style: Double,
                                                        prefix: Empty,
                                                        triple_quoted: false,
                                                        unclosed: false,
                                                    },
                                                },
                                            ),
                                        },
                                    },
                                ),
                            ],
                            ctx: Load,
                        },
                    ),
                ),
                simple: true,
            },
        ),
        Expr(
            StmtExpr {
                node_index: NodeIndex(
                    10,
                ),
                range: 21..43,
                value: List(
                    ExprList {
                        node_index: NodeIndex(
                            11,
                        ),
                        range: 21..43,
                        elts: [
                            Attribute(
                                ExprAttribute {
                                    node_index: NodeIndex(
                                        12,
                                    ),
                                    range: 22..31,
                                    value: Name(
                                        ExprName {
                                            node_index: NodeIndex(
                                                13,
                                            ),
                                            range: 22..26,
                                            id: Name("item"),
                                            ctx: Load,
                                        },
                                    ),
                                    attr: Identifier {
                                        id: Name("for"),
                                        range: 28..31,
                                        node_index: NodeIndex(
                                            14,
                                        ),
                                    },
                                    ctx: Load,
                                },
                            ),
                            Compare(
                                ExprCompare {
                                    node_index: NodeIndex(
                                        15,
                                    ),
                                    range: 32..42,
                                    left: Name(
                                        ExprName {
                                            node_index: NodeIndex(
                                                16,
                                            ),
                                            range: 32..36,
                                            id: Name("item"),
                                            ctx: Load,
                                        },
                                    ),
                                    ops: [
                                        In,
                                    ],
                                    comparators: [
                                        Name(
                                            ExprName {
                                                node_index: NodeIndex(
                                                    17,
                                                ),
                                                range: 40..42,
                                                id: Name("xs"),
                                                ctx: Load,
                                            },
                                        ),
                                    ],
                                },
                            ),
                        ],
                        ctx: Load,
                    },
                ),
            },
        ),
    ],
}
```

Specifically, it seems to be parsed as a list.

To fix this, I think it would be a good idea to see if we can make this specific case parse as a comprehension.

### Version

_No response_

---

_Label `completions` added by @BurntSushi on 2026-01-06 12:39_

---

_Added to milestone `Pre-stable 1` by @BurntSushi on 2026-01-06 12:39_

---

_Comment by @MichaReiser on 2026-01-06 12:55_

> To fix this, I think it would be a good idea to see if we can make this specific case parse as a comprehension.

Agree. The tricky bit here is that we want to disable our "parse a keyword as an expression" recovery logic when parsing the first list element. This probably requires adding some more state: 

One possibility is to add a new flag to `ExpressionContext` e.g. `IN_COMPREHENSION_ELEMENT`, and, if so, skip over the keyword recovery logic in `parse_identifier` if the current identifier is `for`. We might want to add a new `parse_identifier` function so that we don't need to pass `ExpressionContext` at all call sites (seems annoying). 

The alternative is to add the flag to the parser state. But I generally prefer to have local flags over global state.

---

_Assigned to @BurntSushi by @BurntSushi on 2026-01-06 13:06_

---
