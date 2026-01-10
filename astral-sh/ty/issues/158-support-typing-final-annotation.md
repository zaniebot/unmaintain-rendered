```yaml
number: 158
title: support typing.Final annotation
type: issue
state: closed
author: carljm
labels:
  - typing semantics
assignees: []
created_at: 2025-03-26T18:04:01Z
updated_at: 2025-07-22T14:35:45Z
url: https://github.com/astral-sh/ty/issues/158
synced_at: 2026-01-10T02:06:24Z
```

# support typing.Final annotation

---

_Issue opened by @carljm on 2025-03-26 18:04_

Doc: https://docs.python.org/3/library/typing.html#typing.Final
Spec: https://typing.python.org/en/latest/spec/qualifiers.html#uppercase-final


- [x] Infer the type of the RHS for public uses of bare `Final` symbols
- [x] Add a new diagnostic for modifications of `Final`-qualified symbols
- [x] Make sure that both of the above also works for attributes
- [x] Disallow `Final` in function parameter definitions / return-type annotations
- [ ] Prevent overriding of `Final` attributes in subclasses
- [ ] Report an error if there is no binding to a `Final`-qualified symbol
- [ ] Variance inference: if an attribute is marked `Final`, its type should be covariant rather than invariant

Somewhat related tickets: https://github.com/astral-sh/ty/issues/433, https://github.com/astral-sh/ty/issues/529

---

_Renamed from "[red-knot] support typing.Final annotation" to "support typing.Final annotation" by @MichaReiser on 2025-05-07 15:26_

---

_Label `typing semantics` added by @AlexWaygood on 2025-05-11 07:53_

---

_Removed from milestone `GA` by @carljm on 2025-06-11 00:46_

---

_Added to milestone `Beta` by @carljm on 2025-06-11 00:46_

---

_Assigned to @sharkdp by @sharkdp on 2025-07-07 10:05_

---

_Comment by @MichaReiser on 2025-07-22 09:32_

@sharkdp could you update what's left to do in this issue? I think most of it is now handled?

---

_Comment by @sharkdp on 2025-07-22 09:47_

> could you update what's left to do in this issue?

I kept the checklist in the description very much up to date.

> I think most of it is now handled?

I still want to merge https://github.com/astral-sh/ruff/pull/19480 and then I was planning to close this and write follow-up tickets. I think "Prevent overriding of Final attributes in subclasses" is also a core aspect of `Final`, but that could probably be done when we tackle Liskov enforcement, which I think would set up the required functionality anyway(?).

---

_Comment by @sharkdp on 2025-07-22 14:35_

I'm closing this ticket, as we now support the core mechanisms of `typing.Final`. The three remaining items from the description above are now captured in the following tickets:

* https://github.com/astral-sh/ty/issues/871
* https://github.com/astral-sh/ty/issues/872
* https://github.com/astral-sh/ty/issues/488#issuecomment-3103047229

---

_Closed by @sharkdp on 2025-07-22 14:35_

---
