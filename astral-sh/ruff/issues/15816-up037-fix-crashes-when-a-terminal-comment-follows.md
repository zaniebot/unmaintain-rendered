```yaml
number: 15816
title: UP037 fix crashes when a terminal comment follows a literal
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-01-29T20:42:37Z
updated_at: 2025-01-30T06:03:07Z
url: https://github.com/astral-sh/ruff/issues/15816
synced_at: 2026-01-12T15:54:55Z
```

# UP037 fix crashes when a terminal comment follows a literal

---

_@dscorbett_

### Description

The fix for [`quoted-annotation` (UP037)](https://docs.astral.sh/ruff/rules/quoted-annotation/) crashes Ruff 0.9.3 when a quoted annotation ends with a comment and contains a string or number literal, which can occur with `Annotated` or `Literal`.

```console
$ cat >up037.py <<'# EOF'
from __future__ import annotations
from typing import Literal
def f() -> "Literal[0]#":
    return 0
# EOF

$ ruff check --isolated --select UP037 up037.py --diff

error: Fix introduced a syntax error. Reverting all changes.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFix%20error%5D

...quoting the contents of `up037.py`, the rule codes UP037, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!


```

---

_Comment by @charliermarsh on 2025-01-29 20:43_

Why is "contains a string or number literal" required?

---

_Comment by @ntBre on 2025-01-29 21:36_

It looks like a bug with [`SimpleTokenizer`](https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_trivia/src/tokenizer.rs#L503). These are the tokens when the subscript is an identifier (annotation like `"Literal[X]#"`):

```
[
    SimpleToken {
        kind: Name,
        range: 0..7,
    },
    SimpleToken {
        kind: LBracket,
        range: 7..8,
    },
    SimpleToken {
        kind: Name,
        range: 8..9,
    },
    SimpleToken {
        kind: RBracket,
        range: 9..10,
    },
    SimpleToken {
        kind: Comment,
        range: 10..11,
    },
]
```

and this is for the original example:

```
[
    SimpleToken {
        kind: Name,
        range: 0..7,
    },
    SimpleToken {
        kind: LBracket,
        range: 7..8,
    },
    SimpleToken {
        kind: Other,
        range: 8..9,
    },
    SimpleToken {
        kind: Bogus,
        range: 9..11,
    },
]
```

---

_Label `bug` added by @dylwil3 on 2025-01-29 21:46_

---

_Comment by @dylwil3 on 2025-01-29 22:13_

> It looks like a bug with [SimpleTokenizer](https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_trivia/src/tokenizer.rs#L503). These are the tokens when the subscript is an identifier (annotation like "Literal[X]#"):

Not sure if that's a bug or the intended scope for `SimpleTokenizer`.

In any event: we might be able to avoid tokenizing anything, I think, because the tokens for the annotation are available as `checker.tokens()`. In the above example you get:

```
Tokens {
    raw: [
        Name 76..83,
        Lsqb 83..84,
        Int 84..85,
        Rsqb 85..86,
        Comment 86..87,
        Newline 87..87,
    ],
}
```
(not sure why there's a newline but otherwise looks correct)

---

_Label `fixes` added by @AlexWaygood on 2025-01-29 22:35_

---

_Comment by @InSyncWithFoo on 2025-01-29 23:38_

> not sure why there's a newline but otherwise looks correct

I think that's a <em>logical newline</em>:

```rust
// https://github.com/astral-sh/ruff/blob/3125332ec1/crates/ruff_python_parser/src/token.rs#L130-L134
pub enum TokenKind {
    // ...

    /// Token kind for a newline.
    Newline,
    /// Token kind for a newline that is not a logical line break. These are filtered out of
    /// the token stream prior to parsing.
    NonLogicalNewline,

    // ...
}
```

---

_Assigned to @dylwil3 by @dylwil3 on 2025-01-30 00:31_

---

_Closed by @dylwil3 on 2025-01-30 06:03_

---

_Closed by @dylwil3 on 2025-01-30 06:03_

---
