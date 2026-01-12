```yaml
number: 1069
title: stop unioning un-annotated module globals with Unknown
type: issue
state: closed
author: carljm
labels:
  - needs-decision
  - typing semantics
assignees: []
created_at: 2025-08-20T22:01:07Z
updated_at: 2025-10-01T14:40:33Z
url: https://github.com/astral-sh/ty/issues/1069
synced_at: 2026-01-12T15:54:24Z
```

# stop unioning un-annotated module globals with Unknown

---

_@carljm_

Our reasons why we currently union with Unknown are described in https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/doc/public_type_undeclared_symbols.md

While this is technically reasonable, from a gradual guarantee perspective, and consistent with our class-attribute behavior, I think ultimately for module globals, it's the wrong tradeoff for users.

External mutation of module globals is relatively rare, and I don't think it is important that we avoid "false positives" on code that does that. It is fine if you need to add a wider annotation to your module global in this uncommon case, so as to explicitly allow an external mutation you want to be able to do.

On the flip side, un-annotated module globals with unambiguous types that aren't intended for external mutation are common, and requiring them to be annotated or `Final` in order to avoid the union with `Unknown` is just irritating and doesn't add value.

I think the tradeoffs are different for class attributes, where mutation (if you include mutation on instances) is very common. That's not to say we won't ultimately decide to change course there, as well, but I think initially it makes sense to just change this for module globals.

This would also imply no longer unioning with Unknown when unannotated module globals are referenced from a nested scope in the same module.

---

_Added to milestone `Beta` by @carljm on 2025-08-20 22:01_

---

_Label `needs-decision` added by @carljm on 2025-08-20 22:01_

---

_Label `typing semantics` added by @carljm on 2025-08-20 22:01_

---

_Assigned to @carljm by @carljm on 2025-08-22 13:47_

---

_Assigned to @sharkdp by @carljm on 2025-08-22 14:42_

---

_Unassigned @carljm by @sharkdp on 2025-08-22 14:45_

---

_Comment by @carljm on 2025-08-22 15:48_

Three notes on this from discussion with @AlexWaygood :

1. This should imply that we widen inferred literal types (e.g. `x = 3` would infer a public type of `int`)
2. Nested scopes within the same module should see the same type as an import would
3. We should remain open to changing our mind here depending on the mypy-primer report (e.g. if it causes tons of new false positives, for instance from monkeypatching module globals in test suites)

---

_Closed by @sharkdp on 2025-10-01 14:40_

---
