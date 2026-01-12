```yaml
number: 15383
title: "[red-knot] Support `@typing.overload` and overloaded callable types"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2025-01-09T18:55:57Z
updated_at: 2025-11-20T17:45:05Z
url: https://github.com/astral-sh/ruff/issues/15383
synced_at: 2026-01-12T15:54:54Z
```

# [red-knot] Support `@typing.overload` and overloaded callable types

---

_@carljm_

This is a tracking issue to add support for `@typing.overload` callables.

Reference spec: https://typing.python.org/en/latest/spec/overload.html

### Work items

- [x] Represent overloaded callables (functions, methods, etc.) in the type system (https://github.com/astral-sh/ruff/pull/17366)
- [x] Check assignability / consistent subtyping between overloaded callable and other (possibly overloaded) callable types (https://github.com/astral-sh/ruff/pull/17366)
- [ ] Implement other relation methods for overloaded definitions
	- [x] Equivalence (https://github.com/astral-sh/ruff/pull/17698)
	- [x] https://github.com/astral-sh/ty/issues/105
- [x] astral-sh/ty#104
	- [x] Try: handle call semantics to overloaded functions as intersection of callables (https://github.com/astral-sh/ruff/pull/17618)
- [x] astral-sh/ruff#17779
- [ ] ~Update property tests to generate overloaded callables, if possible~ (refer to https://github.com/astral-sh/ruff/issues/15383#issuecomment-2845705716)
- [ ] Raise diagnostic for invalid overloaded definitions
	- [x] If there are < 2 overloaded definitions (https://github.com/astral-sh/ruff/pull/17609)
	- [x] If there is no implementation except in in stub files, protocols, and abstract methods within abstract base classes (https://github.com/astral-sh/ruff/pull/17681)
	- [ ] https://github.com/astral-sh/ty/issues/103
	- [ ] https://github.com/astral-sh/ty/issues/109
	- [x] (_Low priority_) Decorator consistency between overloads and implementation (https://github.com/astral-sh/ruff/pull/17684)
- [ ] ~Check if any of the [special cased overloaded signatures](https://github.com/astral-sh/ruff/blob/44ad2012622d73373eb7922978f391eb6c1e54fa/crates/red_knot_python_semantic/src/types.rs#L3144-L3144) can be removed~ (refer to https://github.com/astral-sh/ruff/issues/15383#issuecomment-2845705716)
- [x] https://github.com/astral-sh/ty/issues/73
	- [ ] How to display overloaded signatures in `reveal_type`, hover, etc.? (See: https://github.com/astral-sh/ty/issues/102)
- [ ] ~Update goto definition for overloads to go to either the matched signature or the implementation~

---

_Label `red-knot` added by @carljm on 2025-01-09 18:55_

---

_Added to milestone `Red Knot Q1 2025` by @carljm on 2025-01-09 18:55_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-03-07 15:05_

---

_Removed from milestone `Red Knot Q1 2025` by @carljm on 2025-03-27 18:33_

---

_Added to milestone `Red Knot Alpha` by @carljm on 2025-03-27 18:33_

---

_Renamed from "[red-knot] support @typing.overload and overloaded callable types" to "[red-knot] Support `@typing.overload` and overloaded callable types" by @dhruvmanila on 2025-04-04 14:29_

---

_Comment by @dhruvmanila on 2025-05-01 20:29_

I've opened individual issues for the remaining tasks for overloads, marking this as complete. The main task which is alpha-worthy is the implementation of the overload call evaluation algorithm but others can be done later on.

I also had a couple of tasks here which I don't think requires a separate issue for the reasons mentioned below.

> Update property tests to generate overloaded callables, if possible

A simple way to do this would just to be to generate a random number of callables between 2-5 and mark them as overload but not provide an implementation because otherwise the implementation would be required to be consistent with the overloads. My main worry here is the soundness of the overloaded definition. It can very well generate an overlapping overload which might create unsoundness which would then create failures in property tests.

> Check if any of the [special cased overloaded signatures](https://github.com/astral-sh/ruff/blob/44ad2012622d73373eb7922978f391eb6c1e54fa/crates/red_knot_python_semantic/src/types.rs#L3144-L3144) can be removed

I looked at a couple of them and the signatures are different than what's present in the typeshed. I think this is for a reason so I'm dropping this for now and something that could be done on a case-by-case basis in the future when we're at a place where these special cases aren't required.

Happy to hear if others have any comments here.

---

_Closed by @dhruvmanila on 2025-05-01 20:37_

---
