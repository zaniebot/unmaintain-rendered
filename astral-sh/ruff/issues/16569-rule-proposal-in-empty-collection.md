```yaml
number: 16569
title: "Rule proposal: `in-empty-collection`"
type: issue
state: closed
author: naslundx
labels:
  - rule
assignees: []
created_at: 2025-03-08T19:53:17Z
updated_at: 2025-05-06T17:48:22Z
url: https://github.com/astral-sh/ruff/issues/16569
synced_at: 2026-01-12T15:54:55Z
```

# Rule proposal: `in-empty-collection`

---

_@naslundx_

### Summary

After discussions in https://github.com/astral-sh/ruff/pull/15732 and https://github.com/astral-sh/ruff/issues/15729 an idea was proposed to add a rule for checking unnecessary membership test in empty collections.

This would reinstate the old behavior in rule PLR6201 to explicitly check for things like `if 1 in []` or `if 'a' not in ''`.

Should ruff include it?

Example implementation is here: #16480 

---

_Renamed from "Rule proposal: InEmptyCollection" to "Rule proposal: `in-empty-collection`" by @MichaReiser on 2025-03-10 11:08_

---

_Label `rule` added by @MichaReiser on 2025-03-10 11:08_

---

_Comment by @MichaReiser on 2025-03-10 11:11_

Unrelated on whether we decide to add this rule. I think I'd prefer naming it `in-empty-sequence` to match python's semantics OR exclude `str` from the rule

> Strings are immutable [sequences](https://docs.python.org/3/library/stdtypes.html#typesseq) of Unicode code points. String literals are written in a variety of ways:
> [source](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)

---

_Comment by @MichaReiser on 2025-03-10 11:12_

I think having this as a rule does make sense. What would you suggest as proposed fix?

---

_Comment by @MichaReiser on 2025-04-28 07:21_

@dylwil3 you asked in #16480 to open this issue. Do you have any concerns with the rule or should we accept this request and land the PR?

---

_Comment by @dylwil3 on 2025-04-28 22:00_

I'm personally happy with the rule, was just checking boxes - sorry for the bureaucratic red tape! @naslundx would you like to sync with main, respond to the remaining comments on the PR and we can get this landed?

---

_Comment by @Avasam on 2025-04-29 00:34_

On the name semantics, `in` is the operator for `__contains__` \* (see message below for caveat), and `typing.Container` is the Protocol for `def __contains__`. So `in-empty-container` ?
That covers strings, lists, tuples, dicts, sets, deque, and I'm sure many other stdlib containers I'm forgetting.
Ok, these are all collections... tbh I'm having a hard time finding a stdlib container that isn't a collection XD

Btw sequences are collections with a few extra methods. So the originally proposed name `in-empty-collection` still works whilst including strings imo.



---

_Comment by @Avasam on 2025-04-29 01:04_

`"" in iter(())` and `"" in iter("")` (ie: `Iterator[Never]`) seem to be in the spirit of this rule, but not a `Container` in the Protocol sense. But `in` also works for Iterables and implementers of `__getitem__()`  https://docs.python.org/3/reference/expressions.html#in

Actually, any empty `<whatever this rule checks for>` passed to `iter` I guess.

---

_Comment by @naslundx on 2025-05-04 17:25_

Not a problem at all @dylwil3 , sorry for dropping this for a while. 

Agree with @Avasam that `collection` seems like the more proper name considering it doesn't just apply to sequences.

I will polish up the PR.

---

_Comment by @dylwil3 on 2025-05-06 17:48_

Resolved in https://github.com/astral-sh/ruff/pull/16480

---

_Closed by @dylwil3 on 2025-05-06 17:48_

---
