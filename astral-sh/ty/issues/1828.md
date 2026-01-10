```yaml
number: 1828
title: "panic: Offset n is inside token FStringMiddle"
type: issue
state: closed
author: correctmost
labels:
  - bug
  - fatal
  - fuzzer
assignees: []
created_at: 2025-12-09T21:47:06Z
updated_at: 2025-12-11T10:45:20Z
url: https://github.com/astral-sh/ty/issues/1828
synced_at: 2026-01-10T01:56:41Z
```

# panic: Offset n is inside token FStringMiddle

---

_Issue opened by @correctmost on 2025-12-09 21:47_

### Summary

ty crashes when checking this fuzzed code:

```python
(c: int = 1,f"""{d=[
def a(
class
```

```
error[panic]: Panicked at crates/ruff_python_ast/src/token/tokens.rs:180:13 when checking `/home/user/fstring.py`: `Offset 28 is inside token `FStringMiddle 27..34 (flags = DOUBLE_QUOTES | TRIPLE_QUOTED_STRING | F_STRING)``
```

### Version

5c02e0b4 w/ astral-sh/ruff@0bec5c0

---

_Label `fatal` added by @AlexWaygood on 2025-12-09 21:51_

---

_Label `fuzzer` added by @AlexWaygood on 2025-12-09 21:51_

---

_Comment by @MichaReiser on 2025-12-09 21:51_

Thanks.

The relevant bits from the stack trace


```
11: ruff_python_ast::token::tokens::Tokens::after
             at /Users/micha/astral/ruff/crates/ruff_python_ast/src/token/tokens.rs:180:13
  12: ruff_python_ast::token::parentheses::parentheses_iterator
             at /Users/micha/astral/ruff/crates/ruff_python_ast/src/token/parentheses.rs:30:16
  13: ty_python_semantic::types::diagnostic::report_invalid_assignment
             at /Users/micha/astral/ruff/crates/ty_python_semantic/src/types/diagnostic.rs:2424:9
  14: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::add_declaration_with_binding
             at /Users/micha/astral/ruff/crates/ty_python_semantic/src/types/infer/builder.rs:1828:21
  15: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_annotated_assignment_definition
             at /Users/micha/astral/ruff/crates/ty_python_semantic/src/types/infer/builder.rs:5846:22
  16: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_region_definition
             at /Users/micha/astral/ruff/crates/ty_python_semantic/src/types/infer/builder.rs:1274:22
  17: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_region
             at /Users/micha/astral/ruff/crates/ty_python_semantic/src/types/infer/builder.rs:520:61
```

---

_Label `bug` added by @AlexWaygood on 2025-12-09 21:53_

---

_Added to milestone `Stable` by @carljm on 2025-12-09 21:54_

---

_Comment by @MichaReiser on 2025-12-09 22:00_

AST

```
Module(
    ModModule {
        node_index: NodeIndex(None),
        range: 0..33,
        body: [
            AnnAssign(
                StmtAnnAssign {
                    node_index: NodeIndex(None),
                    range: 0..28,
                    target: Name(
                        ExprName {
                            node_index: NodeIndex(None),
                            range: 1..2,
                            id: Name("c"),
                            ctx: Store,
                        },
                    ),
                    annotation: Name(
                        ExprName {
                            node_index: NodeIndex(None),
                            range: 4..7,
                            id: Name("int"),
                            ctx: Load,
                        },
                    ),
                    value: Some(
                        Tuple(
                            ExprTuple {
                                node_index: NodeIndex(None),
                                range: 10..28,
                                elts: [
                                    NumberLiteral(
                                        ExprNumberLiteral {
                                            node_index: NodeIndex(None),
                                            range: 10..11,
                                            value: Int(
                                                1,
                                            ),
                                        },
                                    ),
                                    Subscript(
                                        ExprSubscript {
                                            node_index: NodeIndex(None),
                                            range: 12..24,
                                            value: FString(
                                                ExprFString {
                                                    node_index: NodeIndex(None),
                                                    range: 12..19,
                                                    value: FStringValue {
                                                        inner: Single(
                                                            FString(
                                                                FString {
                                                                    range: 12..19,
                                                                    node_index: NodeIndex(None),
                                                                    elements: [
                                                                        Interpolation(
                                                                            InterpolatedElement {
                                                                                range: 16..19,
                                                                                node_index: NodeIndex(None),
                                                                                expression: Name(
                                                                                    ExprName {
                                                                                        node_index: NodeIndex(None),
                                                                                        range: 17..18,
                                                                                        id: Name("d"),
                                                                                        ctx: Load,
                                                                                    },
                                                                                ),
                                                                                debug_text: Some(
                                                                                    DebugText {
                                                                                        leading: "",
                                                                                        trailing: "=",
                                                                                    },
                                                                                ),
                                                                                conversion: None,
                                                                                format_spec: None,
                                                                            },
                                                                        ),
                                                                    ],
                                                                    flags: FStringFlags {
                                                                        quote_style: Double,
                                                                        prefix: Regular,
                                                                        triple_quoted: true,
                                                                        unclosed: true,
                                                                    },
                                                                },
                                                            ),
                                                        ),
                                                    },
                                                },
                                            ),
                                            slice: Slice(
                                                ExprSlice {
                                                    node_index: NodeIndex(None),
                                                    range: 21..24,
                                                    lower: None,
                                                    upper: Some(
                                                        Name(
                                                            ExprName {
                                                                node_index: NodeIndex(None),
                                                                range: 21..24,
                                                                id: Name("def"),
                                                                ctx: Load,
                                                            },
                                                        ),
                                                    ),
                                                    step: None,
                                                },
                                            ),
                                            ctx: Load,
                                        },
                                    ),
                                    Call(
                                        ExprCall {
                                            node_index: NodeIndex(None),
                                            range: 25..27,
                                            func: Name(
                                                ExprName {
                                                    node_index: NodeIndex(None),
                                                    range: 25..26,
                                                    id: Name("a"),
                                                    ctx: Load,
                                                },
                                            ),
                                            arguments: Arguments {
                                                range: 26..27,
                                                node_index: NodeIndex(None),
                                                args: [],
                                                keywords: [],
                                            },
                                        },
                                    ),
                                ],
                                ctx: Load,
                                parenthesized: false,
                            },
                        ),
                    ),
                    simple: false,
                },
            ),
        ],
    },
)
```

Tokens

```
Lpar 0..1
Name 1..2
Colon 2..3
Name 4..7
Equal 8..9
Int 10..11
Comma 11..12
FStringStart 12..16 (flags = DOUBLE_QUOTES | TRIPLE_QUOTED_STRING | F_STRING)
Lbrace 16..17
Name 17..18
Equal 18..19
Lsqb 19..20
NonLogicalNewline 20..21
Def 21..24
Name 25..26
Lpar 26..27
FStringMiddle 27..33 (flags = DOUBLE_QUOTES | TRIPLE_QUOTED_STRING | F_STRING)
Unknown 33..33
Newline 33..33
```

What's interesting here is that we extend the assignment range to include the offset 28 but we skip offer the `FSTringMiddle` token. I'm not sure how we determine the offset 28, since there's no token at that offset. The assignment should either end at offset 27 or 33 but never in-between

---

_Comment by @MichaReiser on 2025-12-10 07:29_

I think the root cause here is that `TokenSource::re_lex_logical_token` edits tokens that the parser has already fully consumed.  I don't think we can do that because it inevitably leads to an inconsistency between the ranges the parser saw and the ranges in the new tokens. Not sure yet what the solution here is

---

_Comment by @MichaReiser on 2025-12-10 08:13_

I think I know how to fix this

---

_Closed by @MichaReiser on 2025-12-11 10:45_

---
