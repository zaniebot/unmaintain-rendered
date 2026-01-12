```yaml
number: 16690
title: "Consider adding a fix for `pytest-raises-ambiguous-pattern` (`RUF043`)"
type: issue
state: open
author: ntBre
labels:
  - fixes
assignees: []
created_at: 2025-03-12T19:00:56Z
updated_at: 2025-06-30T17:52:44Z
url: https://github.com/astral-sh/ruff/issues/16690
synced_at: 2026-01-12T15:54:55Z
```

# Consider adding a fix for `pytest-raises-ambiguous-pattern` (`RUF043`)

---

_@ntBre_

It's certainly a more noisy rule and it's unfortunate we can't provide an auto fix (because Ruff can't know what the desired behavior is). But I think it's fine to stabilize


Unless we want to add a heuristic for the auto fix. E.g.:

* A string ending with `.` is probably a sentence -> escape it
* A string with a sequence of metacharacters (e.g. `.*` or `.+`) is probably a regex -> make it raw. We can limit it to only support some specific patterns.

The first fix would have to be unsafe because it does change the program's semantics. The second one could be safe but I think it's still best to make user explicitly opt-in. We could document the heuristics in the rule and having a fix could reduce the churn for users. 

I'd postpone the promotion to stable if the suggested fix does sound reasonable (CC: @AlexWaygood)

_Originally posted by @MichaReiser in https://github.com/astral-sh/ruff/pull/16657#pullrequestreview-2677569437_
            

---

_Label `fixes` added by @ntBre on 2025-03-12 19:01_

---

_Comment by @AlexWaygood on 2025-03-17 16:39_

We discussed when implementing the rule originally whether it should have an autofix or not and concluded "no" because adding backslash escapes may often be unwelcome (users will often prefer to make it a raw string), but it would probably be hard for Ruff to determine whether a raw string or an escape would be preferred. https://github.com/astral-sh/ruff/pull/14966#issuecomment-2543194280

I also don't _love_ the idea of adding so many heuristics to an autofix; I think it'll be harder for users to understand without reading the docs (and I doubt people go and read the docs until a rule goes and does something that they don't expect).

So I'd be inclined to leave the rule as it is and stabilise it. That said, I don't have a strong opinion; happy to defer to you and @MichaReiser if you think users would find an unsafe fix helpful here!

---

_Comment by @MichaReiser on 2025-03-18 06:48_

I don't feel strongly either and I agree that it can be confusing why the rule only suggests some fixes. However, considering how common the *sentence ending with a dot* is, providing a fix at least for that still seems useful to me. 

> (users will often prefer to make it a raw string), but it would probably be hard for Ruff to determine whether a raw string or an escape would be preferred. https://github.com/astral-sh/ruff/pull/14966#issuecomment-2543194280

I would keep it simple and always suggest `re.escape` because it works for all characters and the entire string was probably not meant to be a regex. However, I can see how this comes down to personal preferences. 



---

_Comment by @DimitriPapadopoulos on 2025-06-29 10:00_

This rule mixes the concepts of [regex metacharacters](https://docs.python.org/3/howto/regex.html#matching-characters) (`.` `^` `$` `*` `+` `?` `{` `}` `[` `]` `\` `|` `(` `)`) and [escape sequences](https://docs.python.org/3/reference/lexical_analysis.html#escape-sequences) (`\`). I find this confusing.

In the documentation, example `match="A full sentence."` should be changed to `("A full sentence\\.")` in most cases, shouldn't it? I don't see how switching to a raw string helps in any way.

```
>>> "A full sentence."
'A full sentence.'
>>> 
>>> r"A full sentence."
'A full sentence.'
>>> 
>>> "A full sentence\\."
'A full sentence\\.'
>>> 
>>> re.escape("A full sentence.")
'A\\ full\\ sentence\\.'
>>> 
```

---

_Comment by @ntBre on 2025-06-30 13:01_

If I remember correctly, the raw string was one of our heuristics for something intentionally being a regex. So you're right that it doesn't change the result, but we were using it as a sign of intent. That could be made more clear in the documentation.

---

_Comment by @DimitriPapadopoulos on 2025-06-30 16:18_

Exactly, I understand it's a convention and a sign of intent. But it's probably a bad idea to add a (safe) fix for a mere sign of intent, especially if the fix changes `"A full sentence."` into `r"A full sentence."`, which is usually not the intent, instead of `re.escape("A full sentence.")`.

---

_Comment by @ntBre on 2025-06-30 17:52_

Oh yes, sorry! I forgot the context of the rest of this thread. I think we're on the same page.

---
