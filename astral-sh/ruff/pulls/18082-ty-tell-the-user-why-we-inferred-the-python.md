```yaml
number: 18082
title: "[ty] Tell the user why we inferred the Python version we inferred"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/tell-me-why-py-version
created_at: 2025-05-14T01:23:45Z
updated_at: 2025-05-21T15:06:29Z
url: https://github.com/astral-sh/ruff/pull/18082
synced_at: 2026-01-10T18:51:01Z
```

# [ty] Tell the user why we inferred the Python version we inferred

---

_Pull request opened by @AlexWaygood on 2025-05-14 01:23_

## Summary

I'm blaming @MichaReiser for this. He said it would be easy in https://github.com/astral-sh/ruff/pull/18068#discussion_r2086919985 and... it wasn't 😆

This PR propagates the origin of the inferred Python version (CLI flag, `pyproject.toml`, `ty.toml`, etc.) from the `ty_project` crate to the `ty_python_semantic` crate. (Specifically, a new `python_version_source` field is added to the `ty_python_semantic::program::Program` struct.) Having the origin of the Python version available allows us to add subdiagnostics that annotate the specific part of the config file that led us to infer a certain Python version.

A screenshot to demonstrate this in action:

![image](https://github.com/user-attachments/assets/21919def-178c-407a-aa5a-2efa32511975)

## Test Plan

I added integration tests to the `ty` crate.


---

_Review requested from @carljm by @AlexWaygood on 2025-05-14 01:23_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-14 01:23_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-14 01:23_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-05-14 01:23_

---

_Label `ty` added by @AlexWaygood on 2025-05-14 01:23_

---

_Label `diagnostics` added by @AlexWaygood on 2025-05-14 01:23_

---

_Comment by @github-actions[bot] on 2025-05-14 01:33_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-05-14 01:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata.rs`:242 on 2025-05-14 05:56_

I'm sorry, but we can't use `db` here. It will break our approach to persistent caching. 

What's important for persistent caching is that we can resolve the `ProgramSettings` before constructing the db. We then hash the `ProgramSettings` and use the hash to lookup the cache file and load the database from it. This allows us to support different persistent caches when running ty with different python versions 

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/value.rs`:143 on 2025-05-14 05:57_

Can't the call site write `value = *ranged` instead?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/program.rs`:19 on 2025-05-14 05:57_

I'd prefer to wrap `PythonVersion` in a struct that stores both the source and the version to reduce the top-level fields and make it harder to only update `python_version` but forget to update the source (which you didn't!)

---

_Review comment by @MichaReiser on `crates/ty_project/src/db.rs`:62 on 2025-05-14 05:58_

See the comment above about persistent caching (refer to my other inline comment)

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata.rs`:20 on 2025-05-14 05:59_

I'd prefer if this doesn't have to be clone. It's a very heavy data structure. It's not clear to me where we have to clone it.


```suggestion
#[derive(Debug, PartialEq, Eq)]
```

---

_@MichaReiser requested changes on 2025-05-14 06:00_

This looks great. Unfortunately, I expect this to break persistent caching where we'd use a different persistent cache for different python versions. 

For now, I think the best we can do, without breaking persistent caching), is to say if the value comes from the CLI or configuration but without saying exactly from which configuration.

---

_Review request for @sharkdp removed by @sharkdp on 2025-05-14 06:35_

---

_Review request for @carljm removed by @carljm on 2025-05-15 02:53_

---

_Comment by @MichaReiser on 2025-05-15 14:40_

Another way to work around the *no db* problem is to store the `SystemPath` in the python-version's `RangedValue` and lazily converting the path to a file when emitting a diagnostic.

---

_@dhruvmanila reviewed on 2025-05-15 15:29_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/program.rs`:19 on 2025-05-15 15:29_

Does this also mean a program with the same Python version but defined at separate source would invalidate the entire program? This isn't that important issue as changing the source of a Python version is not going to be that frequent in practice (I think) and the benefit that it provides is more useful IMO.

---

_@MichaReiser reviewed on 2025-05-15 15:39_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/program.rs`:19 on 2025-05-15 15:39_

We could ignore the source when computing the cache key

---

_Comment by @AlexWaygood on 2025-05-19 17:23_

> Another way to work around the _no db_ problem is to store the `SystemPath` in the python-version's `RangedValue` and lazily converting the path to a file when emitting a diagnostic.

Yeah, I think I'll try this. Having an annotation showing the file where the setting comes from seems really useful to me, since it can already come from one of several possible sources (`pyproject.toml` file or `ty.toml` file -- and when `--config-file` is implemented those could possibly be in unexpected locations) and we intend to add more sources in the future (`pyvenv.cfg` files, maybe also `.python-version` files).

---

_@AlexWaygood reviewed on 2025-05-20 21:11_

---

_Review comment by @AlexWaygood on `crates/ty_project/src/metadata/value.rs`:143 on 2025-05-20 21:11_

`RangedValue` is not itself `Copy`, and the only way to obtain the wrapped `T` data at the moment is the `into_inner` method. But I need to clone the whole `RangedValue` object to use that method, since it consumes `self`. For my callsite, that feels unnecessarily expensive?

---

_Comment by @AlexWaygood on 2025-05-20 22:18_

Thanks @MichaReiser! I think I addressed all your concerns except for https://github.com/astral-sh/ruff/pull/18082#discussion_r2088101404 (I left a comment in response).

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-05-20 22:18_

---

_@MichaReiser reviewed on 2025-05-21 06:16_

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/value.rs`:143 on 2025-05-21 06:16_

I understand. The following works without addidng the new method:

```patch
Index: crates/ty_project/src/metadata/value.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ty_project/src/metadata/value.rs b/crates/ty_project/src/metadata/value.rs
--- a/crates/ty_project/src/metadata/value.rs	(revision 181b122308b8918b284bf3e57a1565a4cb42891a)
+++ b/crates/ty_project/src/metadata/value.rs	(date 1747808036977)
@@ -133,15 +133,6 @@
     }
 }
 
-impl<T> RangedValue<T>
-where
-    T: Copy,
-{
-    pub fn inner_copied(&self) -> T {
-        self.value
-    }
-}
-
 impl<T> Combine for RangedValue<T> {
     fn combine(self, _other: Self) -> Self
     where
Index: crates/ty_project/src/metadata/options.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ty_project/src/metadata/options.rs b/crates/ty_project/src/metadata/options.rs
--- a/crates/ty_project/src/metadata/options.rs	(revision 181b122308b8918b284bf3e57a1565a4cb42891a)
+++ b/crates/ty_project/src/metadata/options.rs	(date 1747808097048)
@@ -98,7 +98,7 @@
             .and_then(|env| env.python_version.as_ref())
             .map(|ranged_version| {
                 PythonVersionWithSource::new(
-                    ranged_version.inner_copied(),
+                    **ranged_version,
                     match ranged_version.source() {
                         ValueSource::Cli => ty_python_semantic::ValueSource::Cli,
                         ValueSource::File(path) => ty_python_semantic::ValueSource::File(
```

`**ranged_version` dereferences the `&ranged_version` to `ranged_version` and then calls `Deref` on `RangedValue`, which returns a `PythonVersion`

---

_Review comment by @MichaReiser on `crates/ty/tests/file_watching.rs`:409 on 2025-05-21 06:18_

We should avoid accessing `options` in general because it doesn't fill in defaults. What's the reason for this change? Can we revert it now that `to_program_settings` only takes a `system`?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/program.rs`:113 on 2025-05-21 06:21_

Nit: Maybe `PythonVersionSource` to make it less ambiguess (identically named structs can be annoying when you do: Go to symbol because they all show up)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/program.rs`:56 on 2025-05-21 06:24_

Nit: I would delete this method and directly use `python_version_with_source` (given that it is public too). It's only used in one place anyway which calls both `python_version` and `python_version_with_source`

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/program.rs`:126 on 2025-05-21 06:25_

It seems that `ProgramSetting's` `Serialize` and `Deserialize` implementation are never used. Can we remove them instead of making this type serializable

---

_@MichaReiser approved on 2025-05-21 06:25_

This is great. Thank you. I've a few smaller inline comments.

---

_@AlexWaygood reviewed on 2025-05-21 14:39_

---

_Review comment by @AlexWaygood on `crates/ty/tests/file_watching.rs`:409 on 2025-05-21 14:39_

thanks, you're right -- these changes are no longer necessary. I'll revert them!

---

_@AlexWaygood reviewed on 2025-05-21 14:40_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/program.rs`:113 on 2025-05-21 14:40_

I wondered if we might start retaining the source for some other settings (e.g. search paths) so that we could also annotate them in diagnostics. But I'll make the renaming you suggest for now (it can easily be renamed in the future if we start using the enum in other contexts)

---

_Review comment by @AlexWaygood on `crates/ty_project/src/metadata/value.rs`:143 on 2025-05-21 14:52_

Ahh, that's a neat trick. Thanks!

---

_@AlexWaygood reviewed on 2025-05-21 14:52_

---

_Merged by @AlexWaygood on 2025-05-21 15:06_

---

_Closed by @AlexWaygood on 2025-05-21 15:06_

---

_Branch deleted on 2025-05-21 15:06_

---
