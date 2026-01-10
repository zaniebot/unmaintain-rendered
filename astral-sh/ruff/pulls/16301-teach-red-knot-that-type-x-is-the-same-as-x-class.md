```yaml
number: 16301
title: "Teach red-knot that `type(x)` is the same as `x.__class__`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/special-case-type
created_at: 2025-02-21T12:21:10Z
updated_at: 2025-02-22T01:41:54Z
url: https://github.com/astral-sh/ruff/pull/16301
synced_at: 2026-01-10T19:49:01Z
```

# Teach red-knot that `type(x)` is the same as `x.__class__`

---

_Pull request opened by @AlexWaygood on 2025-02-21 12:21_

## Summary

I keep trying to write mdtests that use `type(x)` to get the type of `x`, and expecting red-knot to understand that `type(x)` should be inferred as the meta-type of `x`, the same as `x.__class__`. This PR adds the necessary special-casing so that red-knot will now understand this.

## Test Plan

I added a couple of new assertions, integrated into the existing mdtest for `__class__` inference (since it's two ways of accessing the same type). We're also able to remove an existing TODO in an mdtest for `issubclass()` type narrowing as a result of this change.

---

_Label `red-knot` added by @AlexWaygood on 2025-02-21 12:21_

---

_Review requested from @carljm by @AlexWaygood on 2025-02-21 12:21_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-02-21 12:21_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-02-21 12:21_

---

_Comment by @AlexWaygood on 2025-02-21 12:24_

We already have test coverage for three-argument calls to `type` that would fail if we stopped inferring `builtins.type` for those calls, FWIW:

https://github.com/astral-sh/ruff/blob/b3c5932fda77f3df33ad7a8a90133ca1081b7acc/crates/red_knot_python_semantic/resources/mdtest/narrow/isinstance.md?plain=1#L97-L110

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:887 on 2025-02-21 20:49_

"always" is a dangerous word 😆 pretty sure it's possible to break this invariant at runtime with a C extension type. But I also think it's reasonable for us to consider it an invariant.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/issubclass.md`:115 on 2025-02-21 20:50_

I love these unexpected TODO fixes, where multiple features compose together and it just... does the right thing

---

_@carljm approved on 2025-02-21 20:51_

Nice!

---

_@AlexWaygood reviewed on 2025-02-21 20:52_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:887 on 2025-02-21 20:52_

Ugh, really?? I thought that was one of the Big Things that was fixed as part of the 2-to-3 transition 😆 but you probably know better than I!

---

_@carljm reviewed on 2025-02-21 20:54_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:887 on 2025-02-21 20:54_

It's possible I'm wrong and this is no longer possible for C extension types either! Not going to go down the rabbit hole of experimenting with it to find out!!

---

_Comment by @AlexWaygood on 2025-02-21 21:03_

> We already have test coverage for three-argument calls to `type` that would fail if we stopped inferring `builtins.type` for those calls, FWIW:

I changed my mind about this and added a dedicated test. The pre-existing test only tested the three-argument form of `type()` because of the specific setup the test was using (which actually predated our support for parameter types, anyway). I simplified the existing test that used three-argument `type()` at the same time.

---

_Merged by @AlexWaygood on 2025-02-21 21:05_

---

_Closed by @AlexWaygood on 2025-02-21 21:05_

---

_Branch deleted on 2025-02-21 21:05_

---

_@InSyncWithFoo reviewed on 2025-02-22 01:41_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:887 on 2025-02-22 01:41_

This actually doesn't require any C code at all:

```pycon
>>> class C:
... 	__class__ = int
...
>>> type(C())
<class '__main__.C'>
>>> C().__class__
<class 'int'>
```

```pycon
>>> class C:
... 	def __getattribute__(self, attr):
... 		if attr == '__class__':
... 			return int
... 		return super().__getattribute__(attr)
...
>>> type(C())
<class '__main__.C'>
>>> C().__class__
<class 'int'>
```

Another edge case to watch for:

```pycon
>>> class C: ...
... class D: ...
...
>>> c = C()
>>> c.__class__ = D
>>> type(c)
<class '__main__.D'>
>>> c.__class__
<class '__main__.D'>
```

---
