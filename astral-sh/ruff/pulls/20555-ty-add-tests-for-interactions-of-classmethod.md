```yaml
number: 20555
title: "[ty] Add tests for interactions of `@classmethod`, `@staticmethod`, and protocol method members"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/proto-classmethod-staticmethod-tests
created_at: 2025-09-24T16:48:29Z
updated_at: 2025-09-25T11:12:33Z
url: https://github.com/astral-sh/ruff/pull/20555
synced_at: 2026-01-12T15:57:04Z
```

# [ty] Add tests for interactions of `@classmethod`, `@staticmethod`, and protocol method members

---

_@AlexWaygood_

## Summary

This PR adds more tests for interactions between `@classmethod`, `@staticmethod`, and subtyping/assignability checks for protocols with method members. This partially addresses @sharkdp's post-merge review comment in https://github.com/astral-sh/ruff/pull/20165#discussion_r2348190613.

There are a lot of TODOs in these tests that are tricky to fix right now due to the fact that we currently consider function-types as not being fully static if their signature includes a `self` or `cls` argument that isn't annotated. That means that doing tests regarding the type of a method when accessed on the class object itself is currently quite tricky. I expect it to be a lot easier to resolve these TODOs after https://github.com/astral-sh/ruff/pull/20517 has landed (but I also don't think it's urgent to resolve these TODOs, since `@classmethod` and `@staticmethod` members are quite rare).

---

_Review requested from @carljm by @AlexWaygood on 2025-09-24 16:48_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-24 16:48_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-24 16:48_

---

_Label `testing` added by @AlexWaygood on 2025-09-24 16:48_

---

_Label `ty` added by @AlexWaygood on 2025-09-24 16:48_

---

_@sharkdp reviewed on 2025-09-25 07:51_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/protocols.md`:2107 on 2025-09-25 07:51_

I can understand the reasoning, but this seems a bit surprising to me. Can I extend this reasoning to instance methods as well? Does `C` implement the `HasFInstanceMethod` protocol? All type checkers seem to think so, but it does feel wrong to me. Shouldn't there be some constraints on the implicit type of the first parameter that would prevent this?
```py
from typing import Protocol

class HasFInstanceMethod(Protocol):
    def f(self, x: int, /) -> str: ...

class C:
    @classmethod
    def f(cls, *args) -> str:
        return ""

def accept(_: HasFInstanceMethod): ...

accept(C())  # pyright, mypy, pyrefly, ty: fine!
```

---

_@sharkdp approved on 2025-09-25 07:52_

Thank you!

---

_@AlexWaygood reviewed on 2025-09-25 09:13_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/protocols.md`:2107 on 2025-09-25 09:13_

> Does `C` implement the `HasFInstanceMethod` protocol? All type checkers seem to think so, but it does feel wrong to me. Shouldn't there be some constraints on the implicit type of the first parameter that would prevent this?

Yes, I agree with you -- and I added (currently failing) tests in this PR that assert this, too ;) they're up on lines 1791-1795.

The rule I intend to implement for ty is that (given a protocol `P`, a method member `f` and a would-be nominal subtype `N`), two things must be true in order for `N` to satisfy `P`
- the get-type of `f` when accessed on instances of `N` must be consistent with the signature of `f` when accessed on instances of `P`
- the get-type of `f` on `type[N]` must be consistent with the signature of `f` on when accessed on the class object `P`

As you note, this is a bit stricter than any other type checker currently implements, so we'll see how it goes; we might have to back out of this. But I'd like to give it a go, at least.

---

_Merged by @AlexWaygood on 2025-09-25 09:14_

---

_Closed by @AlexWaygood on 2025-09-25 09:14_

---

_Branch deleted on 2025-09-25 09:14_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/protocols.md`:2107 on 2025-09-25 11:05_

> I added (currently failing) tests in this PR that assert this, too ;) they're up on lines 1791-1795.

They looked slightly different to me, because with the `*args` signature in my comment, you can even make type checkers think that the `classmethod` signature *is* compatible with a non-classmethod protocol signature.

---

_@sharkdp reviewed on 2025-09-25 11:05_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/protocols.md`:2107 on 2025-09-25 11:12_

Oh, sorry -- I missed that. Hmm, I can see how that might be counter-intuitive, but yes, I can't immediately see a principled reason whereby we'd decide that `C` shouldn't be a subtype of `HasFInstanceMethod` there...

---

_@AlexWaygood reviewed on 2025-09-25 11:12_

---
