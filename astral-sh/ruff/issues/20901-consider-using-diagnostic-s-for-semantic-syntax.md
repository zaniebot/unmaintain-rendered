```yaml
number: 20901
title: "Consider using `Diagnostic`s for semantic syntax errors"
type: issue
state: open
author: ntBre
labels:
  - needs-design
  - diagnostics
assignees: []
created_at: 2025-10-15T17:47:02Z
updated_at: 2025-10-15T18:08:50Z
url: https://github.com/astral-sh/ruff/issues/20901
synced_at: 2026-01-10T11:10:00Z
```

# Consider using `Diagnostic`s for semantic syntax errors

---

_Issue opened by @ntBre on 2025-10-15 17:47_

While reviewing #20682 today, I  wanted to suggest emitting a diagnostic about alternate `match` patterns binding different variables just like rustc does:

```
error[E0408]: variable `a` is not bound in all patterns
 --> src/main.rs:4:5
  |
4 |     [] | [a, b, c] | [d, e, f]=> {}
  |     ^^    -          ^^^^^^^^^ pattern doesn't bind `a`
  |     |     |
  |     |     variable not in all patterns
  |     pattern doesn't bind `a`
```

However, we don't have direct access to the `Diagnostic` in the `SemanticSyntaxChecker`. Instead, we can currently only `report_semantic_error` to the parent visitor. I don't think we can simply replace `report_semantic_error` with `report_diagnostic`, for example, because Ruff's implementation of `report_semantic_error` needs to convert some semantic errors into rule violations, unlike ty's implementation.

Another wrinkle is that Ruff and ty use a different helper function to convert errors into diagnostics, [`create_syntax_error_diagnostic`](https://github.com/astral-sh/ruff/blob/583ac216c447436db942d6257b60e8d875e9584c/crates/ruff_linter/src/message/mod.rs#L36) vs [`Diagnostic::invalid_syntax`](https://github.com/astral-sh/ruff/blob/583ac216c447436db942d6257b60e8d875e9584c/crates/ruff_db/src/diagnostic/mod.rs#L90). The main difference is where the error message ends up:

```
> ruff check try.py
invalid-syntax: name capture `x` makes remaining patterns unreachable
 --> try.py:2:10
  |
1 | match 42:
2 |     case x: ...
  |          ^
3 |     case y: ...
  |

Found 1 error.
> uvx ty check try.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                                                                 error[invalid-syntax]
 --> try.py:2:10
  |
1 | match 42:
2 |     case x: ...
  |          ^ name capture `x` makes remaining patterns unreachable
3 |     case y: ...
  |

Found 1 diagnostic
```

This is a bit of a separate issue, and I kind of think we should standardize on the Ruff version anyway.

But regardless, it seems useful to get access directly to a `Diagnostic` for semantic syntax errors and possibly for other types of syntax errors too so that we can take advantage of the new functionality instead of converting the various simplified representations into diagnostics later.

---

_Label `needs-design` added by @ntBre on 2025-10-15 17:47_

---

_Label `diagnostics` added by @ntBre on 2025-10-15 17:47_

---

_Comment by @MichaReiser on 2025-10-15 18:01_

Another challenge is that ruff_db depends on ruff_python_parser. So we'd have to remove that dependency first (by moving parsed_module to ruff_python_parser, although we sort of don't want salsa in the parser crate)

---

_Comment by @ntBre on 2025-10-15 18:08_

Oh right, I forgot about that part. I think we could possibly disentangle the diagnostic part from ruff_db, maybe moving it into the ruff_diagnostic crate now that ruff_diagnostics is mostly empty? The `File` type looks like the trickiest part of that.

---
