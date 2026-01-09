---
number: 7517
title: "Update `FormatStringContinuation` to use the new f-string tokens"
type: issue
state: closed
author: dhruvmanila
labels:
  - formatter
  - python312
assignees: []
created_at: 2023-09-19T14:30:44Z
updated_at: 2023-09-25T07:45:56Z
url: https://github.com/astral-sh/ruff/issues/7517
synced_at: 2026-01-07T13:12:15-06:00
---

# Update `FormatStringContinuation` to use the new f-string tokens

---

_Issue opened by @dhruvmanila on 2023-09-19 14:30_

The `FormatStringContinuation` implementation uses the full lexer for comment handling. It expects only a limited set of tokens (`String`, `Newline`, `NonLogicalNewline`, `Comment`, `Indent`, `Dedent`) which is expected given that the lexing is performed only on the string literals.

Now, with PEP 701, the f-string literal will instead emit `FStringStart`, `FStringMiddle`, and `FStringEnd`. Not only that, it'll also emit other tokens for the expression part of the f-string. This poses a challenge as currently it panics for any unexpected token encountered:

https://github.com/astral-sh/ruff/blob/4ae463d04bb65a39aa8af8fa12523cfda04a5d9f/crates/ruff_python_formatter/src/expression/string.rs#L277

---

_Label `formatter` added by @dhruvmanila on 2023-09-19 14:30_

---

_Label `python312` added by @dhruvmanila on 2023-09-19 14:30_

---

_Referenced in [astral-sh/ruff#6502](../../astral-sh/ruff/issues/6502.md) on 2023-09-19 14:39_

---

_Added to milestone `Formatter: Beta` by @dhruvmanila on 2023-09-19 14:40_

---

_Comment by @MichaReiser on 2023-09-19 14:55_

Is this blocking for landing PEP701? 

---

_Comment by @dhruvmanila on 2023-09-19 16:23_

It's one of the failing test in the CI: https://github.com/astral-sh/ruff/actions/runs/6236001637/job/16926357093?pr=7376#step:7:2341

It shouldn't block Ruff in general but the formatter will panic when a f-string is encountered in implicit string concatenation with comments in it. For example,

```python
foo = (
    f"bar"
    # comment
    "foo"
)
```

---

_Comment by @MichaReiser on 2023-09-19 18:11_

Yeah, I don't think we can break the formatter so significantly at this stage 

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-09-22 02:33_

---

_Comment by @dhruvmanila on 2023-09-22 02:34_

I've started looking into it. A basic implementation of using the outermost f-string range and ignoring all tokens within a f-string works but I'll look at if there are any other cases which may break this.

---

_Comment by @dhruvmanila on 2023-09-25 07:45_

Currently, this is completed in a way so as to not block merging PEP 701. Tracking issue for handling comments and quotes in f-string is at #7594 

---

_Closed by @dhruvmanila on 2023-09-25 07:45_

---
