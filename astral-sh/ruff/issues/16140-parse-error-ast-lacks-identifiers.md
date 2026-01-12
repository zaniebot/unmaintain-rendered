```yaml
number: 16140
title: Parse error AST lacks identifiers
type: issue
state: open
author: ndmitchell
labels:
  - parser
assignees: []
created_at: 2025-02-13T15:18:25Z
updated_at: 2025-02-14T13:56:58Z
url: https://github.com/astral-sh/ruff/issues/16140
synced_at: 2026-01-12T15:54:55Z
```

# Parse error AST lacks identifiers

---

_@ndmitchell_

### Description

When using the [parser/AST as a library](https://play.ruff.rs/?secondary=AST), given the expression:

```python
f"{None for y}"
```

The AST produced is equivalent to:

```python
f"{None}"; for EMPTY in EMPTY
```

Both `EMPTY` values are the empty identifier (literally `""`) with the same `range`. I think the first argument to `StmtFor` (the `target`) should be `y` with an associated valid range.

This line also produces 5 parse errors, which seems more than desirable.

---

_Label `parser` added by @AlexWaygood on 2025-02-13 15:19_

---

_Comment by @AlexWaygood on 2025-02-13 15:22_

the full AST we produce here is this:

<details>

```rs
Module(
    ModModule {
        range: 0..15,
        body: [
            Expr(
                StmtExpr {
                    range: 0..7,
                    value: FString(
                        ExprFString {
                            range: 0..7,
                            value: FStringValue {
                                inner: Single(
                                    FString(
                                        FString {
                                            range: 0..7,
                                            elements: [
                                                Expression(
                                                    FStringExpressionElement {
                                                        range: 2..7,
                                                        expression: NoneLiteral(
                                                            ExprNoneLiteral {
                                                                range: 3..7,
                                                            },
                                                        ),
                                                        debug_text: None,
                                                        conversion: None,
                                                        format_spec: None,
                                                    },
                                                ),
                                            ],
                                            flags: FStringFlags {
                                                quote_style: Double,
                                                prefix: Regular,
                                                triple_quoted: false,
                                            },
                                        },
                                    ),
                                ),
                            },
                        },
                    ),
                },
            ),
            For(
                StmtFor {
                    range: 8..11,
                    is_async: false,
                    target: Name(
                        ExprName {
                            range: 11..11,
                            id: Name(""),
                            ctx: Store,
                        },
                    ),
                    iter: Name(
                        ExprName {
                            range: 11..11,
                            id: Name(""),
                            ctx: Invalid,
                        },
                    ),
                    body: [],
                    orelse: [],
                },
            ),
        ],
    },
)
```

</details>

[Playground](https://play.ruff.rs/2137fb5c-315c-4dce-b9a7-ecb0c8483967). I agree that this seems suboptimal. It looks like we drop the `y` name completely from the AST we produce.

---

_Label `bug` added by @MichaReiser on 2025-02-13 15:26_

---

_Label `bug` removed by @MichaReiser on 2025-02-13 15:27_

---

_Comment by @vladNed on 2025-02-14 08:28_

I can try working on this, I do have one question
- I understand that the `target` should be `y`, so the logical path would be that the `iter` remains as an empty value right ? 

---

_Comment by @dhruvmanila on 2025-02-14 08:49_

Related https://github.com/astral-sh/ruff/issues/10653.

I think the reason this must be happening is because the `for` is not being considered the part of the f-string expression but I'm surprised that the parser is dropping `y` because trying it out without the f-string i.e., `None for y` keeps `y` as is.

> * I understand that the `target` should be `y`, so the logical path would be that the `iter` remains as an empty value right ?

Yeah, the recovery isn't ideal in this case but it should be same as `None for y` and avoid dropping `y` from the AST.

---

_Comment by @vladNed on 2025-02-14 10:07_

Debugged a bit the parser and seems like when it hits the `parse_atom` in this particular case, `y` is seen as a `FStringMiddle` 

https://github.com/astral-sh/ruff/blob/63dd68e0edf606bc54afce091e543e5691552f97/crates/ruff_python_parser/src/parser/expression.rs#L519-L522

and because its not yet mapped to a value it will return an empty expression 

https://github.com/astral-sh/ruff/blob/63dd68e0edf606bc54afce091e543e5691552f97/crates/ruff_python_parser/src/parser/expression.rs#L588-L601

This is still due to a syntax error in Python and not sure if it should ever be mapped.

---

_Comment by @dhruvmanila on 2025-02-14 10:14_

> Debugged a bit the parser and seems like when it hits the `parse_atom` in this particular case, `y` is seen as a `FStringMiddle`

Oh, I see. I did not check what the lexer produced and it is indeed `FStringMiddle` (https://play.ruff.rs/b09543de-4176-4c7e-9962-ecb207c095bd?secondary=Tokens), sorry to waste your time.

@ndmitchell I'm curious to know if this is causing any issues on your end. Can you provide some context here?

---

_Comment by @ndmitchell on 2025-02-14 13:56_

No serious issues. Just saw it and it looked a lot worse than all the other error recovery paths. Happy for it go to on the back burner. 

---
