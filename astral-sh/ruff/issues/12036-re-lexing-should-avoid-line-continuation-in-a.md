```yaml
number: 12036
title: Re-lexing should avoid line continuation in a comment
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - parser
assignees: []
created_at: 2024-06-26T03:22:16Z
updated_at: 2024-06-27T11:42:41Z
url: https://github.com/astral-sh/ruff/issues/12036
synced_at: 2026-01-10T11:09:54Z
```

# Re-lexing should avoid line continuation in a comment

---

_Issue opened by @dhruvmanila on 2024-06-26 03:22_

Even with the fix in https://github.com/astral-sh/ruff/pull/12035, we still need to consider the fact that the line continuation character could be part of the comment which means that the newline character is not being escaped.

This won't create a panic but the lexer won't be moved back and keep the `NonLogicalNewline` token instead of changing it to `Newline` token.

For example:
```py
if call(foo # comment \
   def foo():
       pass
```
This will emit the following tokens:
```
If 0..2
Name 3..7
Lpar 7..8
Name 8..11
Comment 12..23
NonLogicalNewline 23..24 <- this should've been a Newline token
Def 28..31
Name 32..35
Lpar 35..36
Rpar 36..37
Colon 37..38
Newline 38..39
Indent 39..47
Pass 47..51
Newline 51..52
Dedent 52..52
```

One solution would be to update `TokenSource` to pass in the last comment token range to re-lexing method on the lexer which can be used to check if this line continuation is part of the comment or not.

This is backwards lexing all over again :)

---

_Label `bug` added by @dhruvmanila on 2024-06-26 03:22_

---

_Label `parser` added by @dhruvmanila on 2024-06-26 03:22_

---

_Comment by @MichaReiser on 2024-06-26 05:28_

Would it help if the lexer remebered the previous token kind? It could then test if the previous kind was a non logical newline and only then do the relexing?

---

_Comment by @dhruvmanila on 2024-06-26 10:49_

As you've mentioned in your [comment in the PR](https://github.com/astral-sh/ruff/pull/12035#pullrequestreview-2140839786), I don't think so that would be sufficient.

---

_Comment by @MichaReiser on 2024-06-26 10:51_

Yeah. I kind of want to avoid that we need to re-implement the entire lexing. Ideally, we could just look at the previous tokens and use that information to make a decision. 

---

_Comment by @dhruvmanila on 2024-06-26 11:49_

> One solution would be to update `TokenSource` to pass in the last comment token range to re-lexing method on the lexer which can be used to check if this line continuation is part of the comment or not.

Do you think this is a better idea? I'm unsure about the split logic.

---

_Comment by @MichaReiser on 2024-06-26 11:55_

I'm open to pass information from the `TokenSource` to the `Lexer`. I'm unsure if it should be the comment because that still requires duplicating the line continuation etc logic. Maybe we could just pass the position of the last `NonLogicalNewline` where last means, the last before any non-trivia token? But I would need to have a closer look at the implementation again to fully understand what information we need. 



---

_Comment by @dhruvmanila on 2024-06-27 03:42_

> Maybe we could just pass the position of the last `NonLogicalNewline` where last means, the last before any non-trivia token?

I literally woke up in the morning thinking about this lol. I think this could work and should simplify the implementation. Let me try it.

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-06-27 03:42_

---

_Closed by @dhruvmanila on 2024-06-27 11:42_

---

_Closed by @dhruvmanila on 2024-06-27 11:42_

---
