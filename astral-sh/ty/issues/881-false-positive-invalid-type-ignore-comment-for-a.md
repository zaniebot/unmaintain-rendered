```yaml
number: 881
title: "False positive `invalid-type-ignore` comment for a comment mentioning `type: ignore` comments"
type: issue
state: closed
author: AlexWaygood
labels:
  - suppression
assignees: []
created_at: 2025-07-24T10:24:00Z
updated_at: 2025-07-24T11:52:43Z
url: https://github.com/astral-sh/ty/issues/881
synced_at: 2026-01-10T02:06:24Z
```

# False positive `invalid-type-ignore` comment for a comment mentioning `type: ignore` comments

---

_Issue opened by @AlexWaygood on 2025-07-24 10:24_

### Summary

Ty doesn't like this block of code in docstring-adder: https://github.com/astral-sh/docstring-adder/blob/1c87636f0f9b3a5f711c645a6cb0b181f7781464/add_docstrings.py#L234-L265

It complains:

```
warning[invalid-ignore-comment]
   --> add_docstrings.py:243:47
    |
241 |             new_body = libcst.IndentedBlock(
242 |                 body=[libcst.SimpleStatementLine(body=[docstring_node])],
243 |                 # but preserve `# type: ignore` comments
    |                                               ^ Invalid `type: ignore` comment: no whitespace after `ignore`
244 |                 header=updated_node.body.trailing_whitespace,
245 |             )
    |
```

A minimal repro of this is:

```py
# make sure to preserve `# type: ignore` comments here:
preserve_the_type_ignore_comments()
```

https://play.ty.dev/7a1ed259-ceb5-49cf-8643-bedec26f82f1

from my perspective as a human, it feels obvious that this is not intended to be a `type: ignore` comment that a tool like ty is meant to parse and recognize :-)

---

_Label `bug` added by @AlexWaygood on 2025-07-24 10:24_

---

_Label `suppression` added by @AlexWaygood on 2025-07-24 10:24_

---

_Comment by @MichaReiser on 2025-07-24 11:27_

Hmm, that'a a bit tricky as it would require detecting balanced backticks. That's why I wouldn't necessarily consider this a bug. A work around here is to write this as 

```py
# but preserve `type: ignore` comments.
```

---

_Comment by @AlexWaygood on 2025-07-24 11:39_

Neither mypy nor pyright considers the comment here an attempt to suppress type-checker diagnostics: neither tool complains that the comment is invalid, nor do they suppress diagnostics on that line if you add the comment

```
# make sure to preserve `# type: ignore` comments here:
```

onto a line that has type errors in it.

The [spec](https://typing.python.org/en/latest/spec/directives.html#type-ignore-comments) says this:

> In some cases, linting tools or other comments may be needed on the same line as a type comment. In these cases, the type comment should be before other comments and linting markers:
>
> ```
>    # type: ignore # <comment or other marker>
> ```
>

That obviously doesn't _forbid_ type checkers from interpreting any `type: ignore` phrase in a comment as an attempt to suppress diagnostics, but it definitely doesn't _mandate_ that either.

---

_Label `bug` removed by @MichaReiser on 2025-07-24 11:48_

---

_Comment by @MichaReiser on 2025-07-24 11:50_

> That obviously doesn't forbid type checkers from interpreting any type: ignore phrase in a comment as an attempt to suppress diagnostics, but it definitely doesn't mandate that either.

We intentionally allow this for `ty: ignore` comments and I don't see a reason to deviate for `type: ignore` comments. 


I think there's an argument whether the `invalid-type-ignore` should be less strict in those cases but I don't think it should. 

I also don't expect that many users will run into this issue. This is only a comment that toolin authors write.

---

_Closed by @AlexWaygood on 2025-07-24 11:52_

---
