```yaml
number: 20347
title: "[ty] Minor fixes to `Protocol` tests"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/proto-test-improvements
created_at: 2025-09-11T11:35:34Z
updated_at: 2025-09-11T14:42:15Z
url: https://github.com/astral-sh/ruff/pull/20347
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Minor fixes to `Protocol` tests

---

_Pull request opened by @AlexWaygood on 2025-09-11 11:35_

_No description provided._

---

_Label `testing` added by @AlexWaygood on 2025-09-11 11:35_

---

_Label `ty` added by @AlexWaygood on 2025-09-11 11:35_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/expression/len.md`:264 on 2025-09-11 11:36_

we could consider emitting a diagnostic on the definition of `__len__` itself here (but that should be a disabled-by-default lint rule, IMO!), but I see no reason why we should emit a diagnostic at the `len()` call itself here. `SecondOptionalArgument` is a subtype of `Sized`.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/functions.md`:80 on 2025-09-11 11:38_

The intent of this test is that `list[str]` should be a subtype of `CanIndex`; below, an instance of `list[str]` is passed into `takes_in_protocol`. But as written, `list[str]` is not a subtype of `CanIndex`, since all parameters of `list.__getitem__` are positional-only

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/protocols.md`:1833 on 2025-09-11 11:38_

A missing test that demonstrates another reason why it's important that dunder methods especially must be looked up on the meta-type when determining protocol subtyping/assignability

---

_@AlexWaygood reviewed on 2025-09-11 11:38_

---

_Marked ready for review by @AlexWaygood on 2025-09-11 11:39_

---

_Review requested from @carljm by @AlexWaygood on 2025-09-11 11:39_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-11 11:39_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-11 11:39_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/protocols.md`:1818 on 2025-09-11 14:33_

```suggestion
and subtyping, we understand that `IterableClass` here is a subtype of `Iterable[int]` even though
`IterableClass.__iter__` has the wrong signature:
```

---

_@carljm approved on 2025-09-11 14:33_

---

_Merged by @AlexWaygood on 2025-09-11 14:42_

---

_Closed by @AlexWaygood on 2025-09-11 14:42_

---

_Branch deleted on 2025-09-11 14:42_

---
