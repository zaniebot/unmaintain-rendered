---
number: 11759
title: Formatter parenthesizes over-long starred expression in an assignment value which prodcues invalid syntax
type: issue
state: closed
author: jasikpark
labels:
  - fuzzer
assignees: []
created_at: 2024-06-05T19:13:35Z
updated_at: 2024-06-10T08:38:47Z
url: https://github.com/astral-sh/ruff/issues/11759
synced_at: 2026-01-07T13:12:15-06:00
---

# Formatter parenthesizes over-long starred expression in an assignment value which prodcues invalid syntax

---

_Issue opened by @jasikpark on 2024-06-05 19:13_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

I ran ` cargo fuzz run ruff_formatter_validity fuzz/artifacts/ruff_formatter_validity/minimized-from-31d7210d32c8aaabd0b146d1dbcb10b8c2184286` on commit `9b2cf569b22855439fa916be6fc417b372074f42`.

Filename:

`fuzz/artifacts/ruff_formatter_validity/minimized-from-31d7210d32c8aaabd0b146d1dbcb10b8c2184286`

File contents:

```
B=*R*VVVVVVVVVRBVVVVVVVVVVVVVVVVVVVVVVVVRBVVVVRBVVVVVVRBVVVVVVVVVVVVVVVVVVVVVVVVVVVRB
```

The crash was 

```
Running: fuzz/artifacts/ruff_formatter_validity/minimized-from-31d7210d32c8aaabd0b146d1dbcb10b8c2184286
thread '<unnamed>' panicked at fuzz_targets/ruff_formatter_validity.rs:65:9:
formatter introduced a parse error
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

https://github.com/astral-sh/ruff/blob/9b2cf569b22855439fa916be6fc417b372074f42/fuzz/fuzz_targets/ruff_formatter_validity.rs#L65-L68

i.e. formatting the above code produces lint errors in code that previously did not produce lint errors.

/cc @MichaReiser 


---

_Comment by @jasikpark on 2024-06-05 19:14_

Initial code:

https://play.ruff.rs/a5f646d9-bce1-468c-bce5-c9d8322e7c98

Formatted once produces lint errors:

https://play.ruff.rs/6e15deb5-03c3-463c-91c6-57a871a17d5e

---

_Renamed from "Formatter/Parser bug" to "Bug: lint error caused by formatting" by @jasikpark on 2024-06-05 19:15_

---

_Comment by @jasikpark on 2024-06-05 19:17_

Original AST:

```
Module(
    ModModule {
        range: 0..85,
        body: [
            Assign(
                StmtAssign {
                    range: 0..85,
                    targets: [
                        Name(
                            ExprName {
                                range: 0..1,
                                id: "B",
                                ctx: Store,
                            },
                        ),
                    ],
                    value: Starred(
                        ExprStarred {
                            range: 2..85,
                            value: BinOp(
                                ExprBinOp {
                                    range: 3..85,
                                    left: Name(
                                        ExprName {
                                            range: 3..4,
                                            id: "R",
                                            ctx: Load,
                                        },
                                    ),
                                    op: Mult,
                                    right: Name(
                                        ExprName {
                                            range: 5..85,
                                            id: "VVVVVVVVVRBVVVVVVVVVVVVVVVVVVVVVVVVRBVVVVRBVVVVVVRBVVVVVVVVVVVVVVVVVVVVVVVVVVVRB",
                                            ctx: Load,
                                        },
                                    ),
                                },
                            ),
                            ctx: Load,
                        },
                    ),
                },
            ),
        ],
    },
)
```

AST after formatting:

```
Module(
    ModModule {
        range: 0..102,
        body: [
            Assign(
                StmtAssign {
                    range: 0..101,
                    targets: [
                        Name(
                            ExprName {
                                range: 0..1,
                                id: "B",
                                ctx: Store,
                            },
                        ),
                    ],
                    value: Starred(
                        ExprStarred {
                            range: 10..99,
                            value: BinOp(
                                ExprBinOp {
                                    range: 11..99,
                                    left: Name(
                                        ExprName {
                                            range: 11..12,
                                            id: "R",
                                            ctx: Load,
                                        },
                                    ),
                                    op: Mult,
                                    right: Name(
                                        ExprName {
                                            range: 19..99,
                                            id: "VVVVVVVVVRBVVVVVVVVVVVVVVVVVVVVVVVVRBVVVVRBVVVVVVRBVVVVVVVVVVVVVVVVVVVVVVVVVVVRB",
                                            ctx: Load,
                                        },
                                    ),
                                },
                            ),
                            ctx: Load,
                        },
                    ),
                },
            ),
        ],
    },
)
```

---

_Label `fuzzer` added by @zanieb on 2024-06-05 19:33_

---

_Referenced in [psf/black#4376](../../psf/black/issues/4376.md) on 2024-06-06 06:17_

---

_Assigned to @MichaReiser by @MichaReiser on 2024-06-06 06:18_

---

_Renamed from "Bug: lint error caused by formatting" to "Formatter parenthesizes over-long starred expression in an assignment value which prodcues invalid syntax" by @MichaReiser on 2024-06-06 06:18_

---

_Comment by @MichaReiser on 2024-06-06 06:20_

A "simple" fix is to change 

```patch
Subject: [PATCH] Track `File` status as input. Implement module resovler and source code as derived queries.
---
Index: crates/ruff_python_formatter/src/expression/expr_starred.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_python_formatter/src/expression/expr_starred.rs b/crates/ruff_python_formatter/src/expression/expr_starred.rs
--- a/crates/ruff_python_formatter/src/expression/expr_starred.rs	(revision 5a5a588a721731e09eb404466649109ff23739be)
+++ b/crates/ruff_python_formatter/src/expression/expr_starred.rs	(date 1717654257492)
@@ -31,6 +31,6 @@
         _parent: AnyNodeRef,
         _context: &PyFormatContext,
     ) -> OptionalParentheses {
-        OptionalParentheses::Multiline
+        OptionalParentheses::Never
     }
 }
```

But I need to validate this in more detail. I also need to go through other places where we parenthesize expressions to ensure starred expressions are valid in that context.

---

_Comment by @MichaReiser on 2024-06-06 06:21_

@dhruvmanila pointed out that the original code isn't valid syntax but it isn't a parse time error

```
> B=*R*VVVVVVVVVRBVVVVVVVVVVVVVVVVVVVVVVVVRBVVVVRBVVVVVVRBVVVVVVVVVVVVVVVVVVVVVVVVVVVRB
  File "<stdin>", line 1
SyntaxError: can't use starred expression here
```

---

_Comment by @jasikpark on 2024-06-06 13:35_

So it's a case where the issue isn't detectable by the linter without an initial formatting then?

---

_Comment by @MichaReiser on 2024-06-06 14:05_

Yeah, it's a syntax error that isn't detected by the parser because it's a compile time syntax error. 

---

_Comment by @jasikpark on 2024-06-06 14:31_

*should* it be detectable, or is this expected for the linter? the ruff_formatter_validity fuzz test may need to be adjusted or at least commented on if it's meant to be allowed

---

_Comment by @MichaReiser on 2024-06-06 14:34_

It's something we may want to detect but isn't something that ruff supports today. It's also an open question if the check should be part of the parser or if it is a deferred linter check

---

_Comment by @jasikpark on 2024-06-06 15:11_

So the fuzzer test is running the linter over code, then formatting it, then checking it a second time if the first lint didn't detect any problems. Is the formatter fundamentally changing the code from something incorrect that the linter can't detect to something incorrect the linter can detect? I don't fully understand python operator precedence, and I guess it doesn't matter since it's invalid code?

---

_Comment by @MichaReiser on 2024-06-06 15:28_

> s the formatter fundamentally changing the code from something incorrect that the linter can't detect to something incorrect the linter can detect?

Yes, the formatter makes a change from an error that the ruff linter should detect (but doesn't today) to something that the parser (comes before linting) detects (already today). 

But the code is in both cases invalid, it's just that Ruff doesn't detect one of the two.

---

_Comment by @jasikpark on 2024-06-06 15:58_

Ah, I see

---

_Comment by @MichaReiser on 2024-06-10 08:38_

While I think we could do better to prevent formatting this snippet in the first place, there's no actionable fix to the formatter that we could implement. 

---

_Closed by @MichaReiser on 2024-06-10 08:38_

---
