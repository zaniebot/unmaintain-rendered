```yaml
number: 12773
title: "`RUF031` (`incorrectly-parenthesized-tuple-in-subscript`) - false positive on types"
type: issue
state: closed
author: DetachHead
labels:
  - duplicate
assignees: []
created_at: 2024-08-09T05:36:34Z
updated_at: 2024-08-09T05:53:49Z
url: https://github.com/astral-sh/ruff/issues/12773
synced_at: 2026-01-12T15:54:52Z
```

# `RUF031` (`incorrectly-parenthesized-tuple-in-subscript`) - false positive on types

---

_@DetachHead_

```toml
# ruff.toml
[lint.ruff]
parenthesize-tuple-in-subscript = true
```
```py
foo = dict[str, str] = {} # error: Use parentheses for tuples in subscripts.
```

subscripts have a completely different meaning in type annotations, and the fact that the arguments to `dict` are a tuple is an implementation detail of the syntax, so i don't think this rule should apply to them.

---

_Comment by @dhruvmanila on 2024-08-09 05:48_

I think this makes sense. I wonder if generics should be accounted here as well. For example:
```py
class Foo(Generic[T1, T2]):
	...
```

@AlexWaygood @MichaReiser Any thoughts?

---

_Label `rule` added by @dhruvmanila on 2024-08-09 05:48_

---

_Label `needs-decision` added by @dhruvmanila on 2024-08-09 05:48_

---

_Comment by @DetachHead on 2024-08-09 05:50_

the same applies in that case too imo, because the subscript syntax is being used to represent generics

---

_Comment by @dhruvmanila on 2024-08-09 05:53_

Oh, I think this is covered by https://github.com/astral-sh/ruff/issues/12758 and a fix for it is already up https://github.com/astral-sh/ruff/pull/12762.

---

_Comment by @dhruvmanila on 2024-08-09 05:53_

(Merging this with https://github.com/astral-sh/ruff/issues/12758)

---

_Closed by @dhruvmanila on 2024-08-09 05:53_

---

_Label `rule` removed by @dhruvmanila on 2024-08-09 05:53_

---

_Label `needs-decision` removed by @dhruvmanila on 2024-08-09 05:53_

---

_Label `duplicate` added by @dhruvmanila on 2024-08-09 05:53_

---
