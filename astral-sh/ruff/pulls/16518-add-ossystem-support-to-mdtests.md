```yaml
number: 16518
title: "Add `OsSystem` support to mdtests"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/mdtest-os-system
created_at: 2025-03-05T12:57:24Z
updated_at: 2025-03-06T15:22:16Z
url: https://github.com/astral-sh/ruff/pull/16518
synced_at: 2026-01-12T15:55:55Z
```

# Add `OsSystem` support to mdtests

---

_@MichaReiser_

## Summary

This PR introduces a new mdtest option `system` that can either be `in-memory` or `os`
where `in-memory` is the default.

The motivation for supporting `os` is so that we can write OS/system specific tests
with mdtests. Specifically, I want to write mdtests for the module resolver, 
testing that module resolution is case sensitive. 

## Test Plan

I tested that the case-sensitive module resolver test start failing when setting `system = "os"`


---

_Label `red-knot` added by @MichaReiser on 2025-03-05 13:01_

---

_@MichaReiser reviewed on 2025-03-05 13:03_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/metadata.rs`:324 on 2025-03-05 13:03_

I'm not very fond of this name. The `MemoryFileSystem::write_files` and `write_file` methods used to create any missing directories. This is inconsistent with what other systems do but is very convenient in tests. 

That's why I renamed the existing `write_files` and `write_file` methods to `write_files_all` and `write_file_all` (in resemblence of `create_dir_all`) and created a new `write_file` method that errors when the parent directory doesn't exist. However, I don't think it's a very good name.

---

_Marked ready for review by @MichaReiser on 2025-03-05 14:38_

---

_Review requested from @carljm by @MichaReiser on 2025-03-05 14:38_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-03-05 14:38_

---

_Review requested from @sharkdp by @MichaReiser on 2025-03-05 14:38_

---

_Comment by @github-actions[bot] on 2025-03-05 14:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @carljm on `crates/red_knot_project/src/files.rs`:258 on 2025-03-05 17:37_

Why is the `as _` needed here?

---

_Review comment by @carljm on `crates/ruff_db/src/system/test.rs`:162 on 2025-03-05 17:48_

Given the naming adjustments throughout this PR, I'm confused why a method named `write_file` explicitly creates all parent directories here.

---

_Review comment by @carljm on `crates/red_knot_project/src/metadata.rs`:324 on 2025-03-05 17:50_

The only other options that come to mind are significantly more verbose; `write_files_mkdirs`, `write_files_with_parents`... I think `write_files_all` is OK.

---

_Review comment by @carljm on `crates/red_knot_test/src/db.rs`:31 on 2025-03-05 18:00_

Do we ever create an `MdtestSystem` with any cwd other than `/`? Should that just be built-in?

---

_Review comment by @carljm on `crates/red_knot_test/src/db.rs`:45 on 2025-03-05 18:01_

It seems surprising at first that "resetting" a real fs just means throwing it away and replacing it with an in-memory fs. But I guess it makes sense.

---

_Review comment by @carljm on `crates/red_knot_test/src/db.rs`:53 on 2025-03-05 18:03_

I think the `with_*` naming is surprising for methods that mutate in-place; that name strongly suggests an immutable builder pattern where `self` is cloned and the updated one is returned. (Because in that case the method name describes the returned object.)

For in-place mutation I would expect e.g. `set_tempdir_fs` and `set_os` instead.

---

_Review comment by @carljm on `crates/red_knot_test/src/db.rs`:40 on 2025-03-05 18:05_

Nit: maybe delegate to a `self.system.reset_fs()` instead of digging so much into `MdTestSystem` internals from outside?

---

_@carljm approved on 2025-03-05 18:07_

Nice!

---

_@MichaReiser reviewed on 2025-03-05 18:55_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/system/test.rs`:162 on 2025-03-05 18:55_

Yeah that's fair. The thinking was that this trait is only used for testing where this seems the right default. But I can see how it is confusing in the bigger picture.

---

_@MichaReiser reviewed on 2025-03-06 09:23_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/files.rs`:258 on 2025-03-06 09:23_

It's not technically needed but it only makes the trait available in the module without importing it by name. That means you can call all the extension methods without binding the name locally.

---

_@MichaReiser reviewed on 2025-03-06 09:28_

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/db.rs`:53 on 2025-03-06 09:28_

>  that name strongly suggests an immutable builder pattern where self is cloned and the updated one is returned

I've seen both usages of `with_` where they take `self` or `&mut self`. I'd say it's even more common for builders taking `self` to omit the `with_` prefix altogether (because they don't have getters). 



---

_Merged by @MichaReiser on 2025-03-06 09:41_

---

_Closed by @MichaReiser on 2025-03-06 09:41_

---

_Branch deleted on 2025-03-06 09:41_

---

_@carljm reviewed on 2025-03-06 15:22_

---

_Review comment by @carljm on `crates/red_knot_test/src/db.rs`:53 on 2025-03-06 15:22_

I think the more relevant distinction maybe is that I would expect builder methods named `with_*` to return `self` (whether they clone or mutate in place) so they can be called chained; again because the `with_*` naming describes what is returned from the method.

---
