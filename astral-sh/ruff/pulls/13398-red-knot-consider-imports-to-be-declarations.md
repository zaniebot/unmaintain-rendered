```yaml
number: 13398
title: "[red-knot] consider imports to be declarations"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/imports-are-declarations
created_at: 2024-09-18T21:48:19Z
updated_at: 2024-09-19T06:04:58Z
url: https://github.com/astral-sh/ruff/pull/13398
synced_at: 2026-01-12T15:55:44Z
```

# [red-knot] consider imports to be declarations

---

_@carljm_

I noticed that this pattern sometimes occurs in typeshed:
```
if ...:
    from foo import bar
else:
    def bar(): ...
```

If we have the rule that symbols with declarations only use declarations for the public type, then this ends up resolving as `Unknown | Literal[bar]`, because we didn't consider the import to be a declaration.

I think the most straightforward thing here is to also consider imports as declarations. The same rationale applies as for function and class definitions: if you shadow an import, you should have to explicitly shadow with an annotation, rather than just doing it implicitly/accidentally.

We may also ultimately need to re-evaluate the rule that public type considers only declarations, if there are declarations.

---

_Label `red-knot` added by @carljm on 2024-09-18 21:48_

---

_Review requested from @MichaReiser by @carljm on 2024-09-18 21:48_

---

_Review requested from @AlexWaygood by @carljm on 2024-09-18 21:48_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:5739 on 2024-09-18 22:14_

Lol. Yeah, I think any type that's not a builtin (or that doesn't have `__main__` as its `__module__` at runtime? not sure) would probably benefit from having the qualified name printed in all cases

---

_Comment by @github-actions[bot] on 2024-09-18 22:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-09-19 01:12_

---

_@carljm reviewed on 2024-09-19 01:20_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:5739 on 2024-09-19 01:20_

I had a PR for this a while back, but it's actually pretty noisy to do that. It seems like pyright does something more clever where it only outputs the module name when it's actually relevant to disambiguate. I haven't looked into the details of how that's implemented. 

---

_@carljm reviewed on 2024-09-19 01:22_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:5739 on 2024-09-19 01:22_

It might be ok to include the module name for any non-builtin function or class defined outside the current module?

---

_Merged by @carljm on 2024-09-19 03:59_

---

_Closed by @carljm on 2024-09-19 03:59_

---

_Branch deleted on 2024-09-19 03:59_

---

_@MichaReiser reviewed on 2024-09-19 06:04_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:5739 on 2024-09-19 06:04_

Feels very noisy unless there's another symbol with the same name in the scope. 

We may also decide that we don't want this to be part of `Display`. E.g. VS code and other editors show a type's import path as separate information from its name.

---
