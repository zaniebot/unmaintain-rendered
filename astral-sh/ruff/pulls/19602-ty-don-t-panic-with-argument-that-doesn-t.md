```yaml
number: 19602
title: "[ty] Don't panic with argument that doesn't actually implement Iterable"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/fix-the-glitch
created_at: 2025-07-28T14:37:40Z
updated_at: 2025-07-28T16:09:56Z
url: https://github.com/astral-sh/ruff/pull/19602
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Don't panic with argument that doesn't actually implement Iterable

---

_Pull request opened by @dcreager on 2025-07-28 14:37_

This eliminates the panic reported in https://github.com/astral-sh/ty/issues/909, though it doesn't address the underlying cause, which is that we aren't yet checking the types of the fields of a protocol when checking whether a class implements the protocol. And in particular, if a class explictly opts out of iteration via

```py
class NotIterable:
    __iter__ = None
```

we currently treat that as "having an `__iter__`" member, and therefore implementing `Iterable`.

Note that the assumption that was in the comment before is still correct: call binding will have already checked that the argument satisfies `Iterable`, and so it shouldn't be an error to iterate over said argument. But arguably, the new logic in this PR is a better way to discharge that assumption — instead of panicking if we happen to be wrong, fall back on an unknown iteration result.

---

_Review requested from @carljm by @dcreager on 2025-07-28 14:37_

---

_Review requested from @AlexWaygood by @dcreager on 2025-07-28 14:37_

---

_Review requested from @sharkdp by @dcreager on 2025-07-28 14:37_

---

_Comment by @github-actions[bot] on 2025-07-28 14:39_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_@AlexWaygood approved on 2025-07-28 14:40_

I'd prefer it if we could emit a tracing warning to say that the invariant has been broken before falling back to `Unknown` here. But I agree that we should get rid of the `.expect()` for now, until our `Protocol` implementation is more robust

---

_Label `ty` added by @AlexWaygood on 2025-07-28 14:40_

---

_Comment by @github-actions[bot] on 2025-07-28 14:42_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @MichaReiser on 2025-07-28 14:42_

> I'd prefer it if we could emit a tracing warning to say that the invariant has been broken before falling back to Unknown here. But I agree that we should get rid of the .expect() for now, until our Protocol implementation is more robust

Not sure if you implied a tracing message with severity `warning`. But this message should only use `debug` because there isn't anything a user can do about it. As I understand it, it's more a warning for us devs.


---

_Comment by @dcreager on 2025-07-28 14:43_

> As I understand it, it's more a warning for us devs.

That's correct, it's for us. If we want something noisy to indicate to us that our semantics are broken, arguably that's exactly what the panic was for. Would it be better to just leave that in place?

---

_Comment by @AlexWaygood on 2025-07-28 14:45_

> If we want something noisy to indicate to us that our semantics are broken, arguably that's exactly what the panic was for.

yes, that's why I added the `.expect()` call in the first place! But given that this is the second crash report we've received about it (the first one was https://github.com/astral-sh/ty/issues/764), I think we should get rid of it for now.

A `debug`-level trace sounds good to me

---

_Comment by @AlexWaygood on 2025-07-28 14:48_

Essentially my opinion is: ideally we will at some point add this `.expect()` call back (it's obviously been pretty good so far at pointing out places where our invariants have not held 😆) but right now it seems too early to have it there. It's just failing too often right now, due to known missing features.

---

_Comment by @dcreager on 2025-07-28 14:51_

> A `debug`-level trace sounds good to me

Done

---

_@AlexWaygood approved on 2025-07-28 14:53_

---

_Merged by @dcreager on 2025-07-28 16:09_

---

_Closed by @dcreager on 2025-07-28 16:09_

---

_Branch deleted on 2025-07-28 16:09_

---
