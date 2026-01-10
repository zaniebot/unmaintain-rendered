```yaml
number: 19243
title: "[ty] Improve and document equivalence for module-literal types"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/moduletype-equivalence
created_at: 2025-07-09T18:27:59Z
updated_at: 2025-07-10T09:11:11Z
url: https://github.com/astral-sh/ruff/pull/19243
synced_at: 2026-01-10T18:33:12Z
```

# [ty] Improve and document equivalence for module-literal types

---

_Pull request opened by @AlexWaygood on 2025-07-09 18:27_

## Summary

Fixes https://github.com/astral-sh/ty/issues/790. For single-file modules (that could never have submodules), we no longer store which file imported the module on the `ModuleLiteralType` struct itself. This means that `<module 'typing'>` is always equivalent to `<module 'typing'>`, and `<module 'typing'> | int | str` is always equivalent to `int | str | <module 'typing'>`.

Module-literal types representing packages are still not considered equivalent types if they were originally imported in different modules, because we would not treat the types equivalently! Tests are added demonstrating why this needs to be the case.

## Test Plan

- mdtests
- Ran `QUICKCHECK_TESTS=100000 cargo test --release -p ty_python_semantic -- --ignored types::property_tests::stable`


---

_Label `ty` added by @AlexWaygood on 2025-07-09 18:29_

---

_Comment by @github-actions[bot] on 2025-07-09 18:31_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
meson (https://github.com/mesonbuild/meson)
- error[invalid-assignment] unittests/failuretests.py:49:5: Object of type `def new_which(cmd, *kwargs) -> Unknown` is not assignable to attribute `which` on type `<module 'shutil'> | <module 'shutil'>`
+ error[invalid-assignment] unittests/failuretests.py:49:5: Implicit shadowing of function `which`

```
</details>
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     memo metadata = ~21MB
+     memo metadata = ~20MB
-     memo fields = ~159MB
+     memo fields = ~152MB

sphinx (https://github.com/sphinx-doc/sphinx)
-     struct fields = ~15MB
+     struct fields = ~14MB

prefect (https://github.com/PrefectHQ/prefect)
-     struct metadata = ~25MB
+     struct metadata = ~23MB

```
</details>


---

_Comment by @AlexWaygood on 2025-07-09 18:33_

Mypy_primer indicates that this leads to a small reduction in memory usage and fewer error messages that mention confusing types such as `<module 'shutil'> | <module 'shutil'>` 😆

---

_Marked ready for review by @AlexWaygood on 2025-07-09 18:41_

---

_Review requested from @carljm by @AlexWaygood on 2025-07-09 18:41_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-07-09 18:41_

---

_Review requested from @dcreager by @AlexWaygood on 2025-07-09 18:41_

---

_@MichaReiser reviewed on 2025-07-10 05:54_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:7511 on 2025-07-10 05:54_

Do we need to store this on `ModuleLiteralType`, given that the kind is also available as `self.module.kind().is_package()` (and said enum is exactly the same).

Oh, I see you attach the `importing_file` if it's a package. I wonder if it would make more sense to make the field an `Option<File>` instead and document when the field is initialized. I think that makes it less confusing because it doesn't look like we store the same information twice.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:7511 on 2025-07-10 06:08_

> Oh, I see you attach the `importing_file` if it's a package. I wonder if it would make more sense to make the field an `Option<File>` instead and document when the field is initialized. I think that makes it less confusing because it doesn't look like we store the same information twice.

I considered that. I went with this way because I thought it made it easier to see in which situations we store `importing_file` and why. But I agree it's confusing in its own way; I'll switch to your suggestion!

---

_@AlexWaygood reviewed on 2025-07-10 06:08_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-07-10 08:59_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:7534 on 2025-07-10 09:01_

I'm not sure this debug assertion is worth it. It's more complicated than the actual code and it's not like the initialization is spread over many places. 

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:7534 on 2025-07-10 09:02_

```suggestion
        debug_assert_eq!(
            self._importing_file(db).is_some(), self.module(db).kind().is_package()
        );
```

---

_@MichaReiser approved on 2025-07-10 09:02_

---

_Merged by @AlexWaygood on 2025-07-10 09:11_

---

_Closed by @AlexWaygood on 2025-07-10 09:11_

---

_Branch deleted on 2025-07-10 09:11_

---
