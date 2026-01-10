```yaml
number: 569
title: Be less assertive about unknown types not being iterable
type: issue
state: closed
author: JacobCallahan
labels:
  - needs-info
assignees: []
created_at: 2025-06-02T18:08:07Z
updated_at: 2025-06-02T19:44:53Z
url: https://github.com/astral-sh/ty/issues/569
synced_at: 2026-01-10T02:34:10Z
```

# Be less assertive about unknown types not being iterable

---

_Issue opened by @JacobCallahan on 2025-06-02 18:08_

I believe there should be a distinction between known not-iterable conditions and unknown not-iterable conditions.

If you see the screenshot below, the type is not known to `ty`, but ty gives an error about the object maybe not being iterable. This is different than the warning seen above where `ty` is unsure about the attribute being unbound, so gives a warning.

![Image](https://github.com/user-attachments/assets/3af918c6-91fb-4bef-aa57-972221583e5a)

It would be nice to have a `possibly-not-iterable` rule to fit cases like these to distinguish between known and unknown iterable states.

```
ty 0.0.1-alpha.8
```

---

_Label `needs-info` added by @carljm on 2025-06-02 18:24_

---

_Comment by @carljm on 2025-06-02 18:24_

Thanks for the report!

Perhaps the diagnostic could be improved to make this clearer, but in both of these cases the type that ty is worried about is not the `Unknown`, it's the `None`. ty is very forgiving about `Unknown` -- will happily consider it iterable and having all attributes. But `None` is a known quantity: it is definitely not iterable, it definitely does not  have an `info` attribute, so these are correct errors for ty to emit.

Whether it's correct for ty to believe that `logger` and `providers` in these two snippets can be `None`, is a different question, but it's one that can't be answered with just the bits of code you included here.

---

_Comment by @JacobCallahan on 2025-06-02 19:44_

@carljm that's a great explanation of `ty`'s point of view on this. Looking at the larger context, I see that the parameter is indeed unguarded at this point to make sure the value isn't `None` in this comprehension. Once that guarding takes place, the error message is removed!

With that in mind, I would consider `ty`'s behavior to be the most correct assessment. Thanks, Astral team, for another great tool!

---

_Closed by @JacobCallahan on 2025-06-02 19:44_

---
