```yaml
number: 14998
title: "[red-knot] Understand other legacy `typing` aliases"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - ty
assignees: []
merged: true
base: main
head: rk-typing-aliases
created_at: 2024-12-16T00:20:19Z
updated_at: 2024-12-17T09:38:31Z
url: https://github.com/astral-sh/ruff/pull/14998
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Understand other legacy `typing` aliases

---

_Pull request opened by @InSyncWithFoo on 2024-12-16 00:20_

## Summary

Resolves #14997.

## Test Plan

Markdown tests.


---

_Review requested from @carljm by @InSyncWithFoo on 2024-12-16 00:20_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2024-12-16 00:20_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2024-12-16 00:20_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2024-12-16 00:20_

---

_Comment by @github-actions[bot] on 2024-12-16 00:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @MichaReiser on 2024-12-16 07:31_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/stdlib_typing_aliases.md`:96 on 2024-12-16 14:56_

Can you resolve this TODO as well, while we're here? It should be trivial, given the other changes you've already made as part of this PR

---

_@AlexWaygood reviewed on 2024-12-16 14:57_

Thanks. I think we'll probably have to rewrite a lot of this when we add support for generics, but I suppose there's no harm in improving our understanding of these annotations as best we can in the meantime

---

_@AlexWaygood reviewed on 2024-12-16 14:58_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/stdlib_typing_aliases.md`:96 on 2024-12-16 14:58_

you just need to fill in this branch of the `match` statement: https://github.com/astral-sh/ruff/blob/a623d8f7c44cd63897489cad0faed1538d4373c8/crates/red_knot_python_semantic/src/types/class_base.rs#L115-L123

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class_base.rs`:132 on 2024-12-16 17:47_

```suggestion
                    todo_type!("Support for Callable as a base class"),
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/stdlib_typing_aliases.md`:71 on 2024-12-16 17:48_

Nit: can you give these subclasses slightly more descriptive names, e.g.

```suggestion
class ListSubclass(typing.List): ...
```

---

_@AlexWaygood reviewed on 2024-12-16 17:48_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/stdlib_typing_aliases.md`:106 on 2024-12-16 19:03_

since we'll just be taking our information from typeshed:

```suggestion
# TODO: Should be (CounterSubclass, Counter, dict, MutableMapping, Mapping, Collection, Sized, Iterable, Container, Generic, object)
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/stdlib_typing_aliases.md`:112 on 2024-12-16 19:03_

```suggestion
# TODO: Should be (DefaultDictSubclass, defaultdict, dict, MutableMapping, Mapping, Collection, Sized, Iterable, Container, Generic, object)
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/stdlib_typing_aliases.md`:118 on 2024-12-16 19:06_

```suggestion
# TODO: Should be (DequeSubclass, deque, MutableSequence, Sequence, Reversible, Collection, Sized, Iterable, Container, Generic, object)
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/stdlib_typing_aliases.md`:124 on 2024-12-16 19:06_

```suggestion
# TODO: Should be (OrderedDictSubclass, OrderedDict, dict, MutableMapping, Mapping, Collection, Sized, Iterable, Container, Generic, object)
```

---

_@AlexWaygood approved on 2024-12-16 19:07_

LGTM

---

_Merged by @AlexWaygood on 2024-12-17 09:33_

---

_Closed by @AlexWaygood on 2024-12-17 09:33_

---

_Branch deleted on 2024-12-17 09:38_

---
