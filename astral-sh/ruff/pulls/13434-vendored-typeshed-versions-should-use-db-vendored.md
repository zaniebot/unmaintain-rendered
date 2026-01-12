```yaml
number: 13434
title: "`vendored_typeshed_versions` should use `db.vendored`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/use-vendored-fs-from-db
created_at: 2024-09-21T09:20:53Z
updated_at: 2024-09-21T14:35:09Z
url: https://github.com/astral-sh/ruff/pull/13434
synced_at: 2026-01-12T15:55:44Z
```

# `vendored_typeshed_versions` should use `db.vendored`

---

_@MichaReiser_

## Summary

This fixes a bug in the module resolver where it always defaulted to the versions file
bundled with Red Knot rather than using the vendored file system instance returned by `db.vendored`. 

Not using `db.vendored` can lead to inconsistencies, as seen in the ruff module graph resolver where
the module resolver loads the versions file from the static vendored file system but resolves the modules from the empty vendored file system set on the `ModuleGraphDb`. 


Now, this also shows that Ruff **needs** the vendored stubs. At least, the VERSIONS file because Red Knot verifies if the stub directory is valid when constructing the `ProgramSettings` to prevent the situation where a user runs `red_knot fix` with a broken custom typeshed dir.


## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2024-09-21 09:20_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-09-21 09:20_

---

_Label `red-knot` added by @MichaReiser on 2024-09-21 09:20_

---

_Comment by @github-actions[bot] on 2024-09-21 09:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-09-21 09:40_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:209 on 2024-09-21 09:40_

I think it's fine to defer validating the vendored typeshed versions. They should be valid and whether we panic here or when trying to resolve the first module doesn't make a real difference. It results in a panic.

The only downside is that getting the versions is now a salsa lookup rather than a static field access. Let's see if this hurts performance in a meaningful way

---

_@MichaReiser reviewed on 2024-09-21 09:45_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:286 on 2024-09-21 09:45_

Not sure why I didn't use `Cow` for this... :face_with_diagonal_mouth: 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_resolver/resolver.rs`:209 on 2024-09-21 09:53_

Okay, there's a 1% performance drop. That doesn't seem worth it. 

I'm just gonna make `vendored_typeshed_versions` always parse the file without caching. This is an overall simplification and shouldn't hurt performance except in the case when we reconstruct the `SearchPathSettings` because the project configuration has changed.... but that's a very slow path anyway and parsing the VERSIONS file should be rather fast

---

_@MichaReiser reviewed on 2024-09-21 09:53_

---

_Comment by @MichaReiser on 2024-09-21 09:55_

I tried to make the typeshed versions usage as lazy as possible but there are some module graph tests that still access typeshed for imports that fail to resolve. We would have to add support for **no typeshed** to the module resolver to make the vendored stubs truly optional.

---

_Merged by @MichaReiser on 2024-09-21 14:35_

---

_Closed by @MichaReiser on 2024-09-21 14:35_

---

_Branch deleted on 2024-09-21 14:35_

---
