---
number: 22160
title: PLR0124 Documentation Issue
type: issue
state: open
author: bruno-brant
labels:
  - documentation
assignees: []
created_at: 2025-12-23T11:12:53Z
updated_at: 2025-12-26T08:51:24Z
url: https://github.com/astral-sh/ruff/issues/22160
synced_at: 2026-01-10T01:23:02Z
---

# PLR0124 Documentation Issue

---

_Issue opened by @bruno-brant on 2025-12-23 11:12_

### Summary

PLR0124 documentation states:

> Comparing a name to itself always results in the same value (...)

Bu it doesn't, as one can overload `__eq__`.

This came up in a situation where I had an assertion like:

```python
assert foo == foo
```

I wrote the test because foo's class had overriden __eq__ and I needed to test that the equality still held. 

I suggest to rewrite the first sentence, switching "always" to "likely" and adjusting the rest accordingly:

> Comparing a name to itself likely results in a truthy value, and is probably a mistake.


### Version

N/A

---

_Label `documentation` added by @ntBre on 2025-12-23 14:08_

---

_Comment by @ntBre on 2025-12-23 14:19_

Hmm, I think I initially read this as "Comparing a name to itself **is always true**, and is likely a mistake," but it actually says "always results in the same value." That kind of raises the required strangeness level of an `__eq__` implementation that would break this assumption (i.e. `foo == foo` is sometimes true and sometimes false).

It's probably still worth calling out in the docs, but I'd probably make it more of a footnote or at least separate section rather than changing the main text since this should be rare.

---

_Comment by @bruno-brant on 2025-12-23 17:33_

Hi @ntBre!

Indeed, a symbol should always equal itself; but we are not talking about the implementation of the equality operation but the scrutinization of code syntax. Given a python object can have a custom __eq__ that can perform whatever the user desires, stating "always results in the same value" is false - that's implementation dependant. 

It's nitpicking, for sure, but personally I would prefer the docs to be more precise. Maybe adding "should" (as in _should always result in the same value_) would suffice? Not giving the absolute idea that this is automatically true?

---

_Comment by @MichaReiser on 2025-12-24 19:20_

I agree with @ntBre that this seems like a very specific edge case that I don't think we need to highlight so prominently. But a prominent case where this is true is comparing `math.nan` with itself.

---

_Comment by @bruno-brant on 2025-12-24 21:39_

Is it proeminent to include a _should_ qualifier? Genuinely asking, sorry if I'm sounding pushy. 

I would agree with a note to at least make sure users understand that this is a call to `__eq__`. 

---

_Comment by @MichaReiser on 2025-12-26 08:51_

I wouldn't mind changing always to something like: "for most objects, comparing it to itself..." 

---
