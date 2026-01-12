```yaml
number: 16170
title: "Use `ast::PythonVersion` internally in the formatter and linter"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
assignees: []
merged: true
base: main
head: brent/formatter-version
created_at: 2025-02-14T20:48:49Z
updated_at: 2025-02-18T17:03:16Z
url: https://github.com/astral-sh/ruff/pull/16170
synced_at: 2026-01-12T15:55:53Z
```

# Use `ast::PythonVersion` internally in the formatter and linter

---

_@ntBre_

## Summary

This PR updates the formatter and linter to use the `PythonVersion` struct from the `ruff_python_ast` crate internally. While this doesn't remove the need for the `linter::PythonVersion` enum, it does remove the `formatter::PythonVersion` enum and limits the use in the linter to deserializing from CLI arguments and config files and moves most of the remaining methods to the `ast::PythonVersion` struct.

## Test Plan

Existing tests, with some inputs and outputs updated to reflect the new (de)serialization format. I think these are test-specific and shouldn't affect any external (de)serialization.


---

_Label `internal` added by @ntBre on 2025-02-14 20:49_

---

_Comment by @ntBre on 2025-02-14 20:57_

Apparently I also messed up some cargo features here, like those that caused #16169. Sorry Micha! I guess these pass locally because I always run clippy and nextest on the whole workspace.

---

_@MichaReiser reviewed on 2025-02-17 07:45_

---

_Review comment by @MichaReiser on `crates/ruff/tests/snapshots/show_settings__display_default_settings.snap`:192 on 2025-02-17 07:45_

I think this should serialize to "3.7" instead to have it symmetric with the deserialization.

---

_@MichaReiser reviewed on 2025-02-17 07:48_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/python_version.rs`:99 on 2025-02-17 07:48_

I think we should move this out of the ast crate. pep440 is only relevant when using pyproject.tomls (I consider this another boundary API whereas `PythonVersion` is our internal representation)

---

_@MichaReiser reviewed on 2025-02-17 07:54_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/python_version.rs`:199 on 2025-02-17 07:54_

I guess that's a work around for serde's lack of `TryFrom` and `Into` on the field level? 

I'm inclined to move those into `serde` and rename them to `deserialize_into`, `try_serialize_from`, `deserialize_into` 

An alternative, which would also remove the need for `deserialize_option`, `serialize_option` and potential `deserialize_*` variants for any other type parametrized with `PythonVersion` is to introduce a newtype wrapper around `PythonVersion` in `ruff_workspace` and attribute that newtype wrapper with serde's [try_from](https://serde.rs/container-attrs.html#try_from) or implement the logic right on that type.

---

_@MichaReiser reviewed on 2025-02-17 07:57_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/options.rs`:27 on 2025-02-17 07:57_

Is this type used anywhere in a user facing facade? If not, I then think it's fine if the representation changes.

---

_@MichaReiser reviewed on 2025-02-17 07:59_

---

_Review comment by @MichaReiser on `ruff.schema.json`:2777 on 2025-02-17 07:59_

You also have to override the schema. Overall, this makes me believe we should keep the old enum in `ruff_workspace` and only convert to the internal `PythonVersion` when going form `Options` (extenral) to `Configurations` (internal)

---

_@ntBre reviewed on 2025-02-17 13:09_

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/python_version.rs`:199 on 2025-02-17 13:09_

Yes that's exactly right. I named the first two `serialize` and `deserialize` because you're supposed to be able to pass a whole module to the [`with`](https://serde.rs/field-attrs.html#with) attribute if they have those names, but I had to specify the generics and couldn't use `with` anyway. And then I still couldn't reuse the code for `Option`s because `Option<T>` doesn't implement `Into<Option<U>>` even if `T: Into<U>`.

I was hesitant to add a new type when I was trying to remove types, but I think you're right that it would be more elegant. I'll give it a try!

---

_@ntBre reviewed on 2025-02-17 13:12_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/options.rs`:27 on 2025-02-17 13:12_

It comes out in `ruff check --show-setttings`, but the version that's printed now (`Py39`) can't be directly pasted back into a config file anyway, so I think it should be fine, like you said.

---

_@MichaReiser reviewed on 2025-02-17 13:22_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/python_version.rs`:199 on 2025-02-17 13:22_

My main goal is to reduce the `PythonVersion` used internally. I do think it's okay to have specific types that are specific to the user interface (that get converted to the internal representation at the API boundaries)

---

_@ntBre reviewed on 2025-02-17 13:38_

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/python_version.rs`:99 on 2025-02-17 13:38_

That makes sense, I put it back on the `linter::PythonVersion` enum.

---

_@ntBre reviewed on 2025-02-17 13:39_

---

_Review comment by @ntBre on `ruff.schema.json`:2777 on 2025-02-17 13:39_

This, combined with the pep440 change, made things much easier. Thank you!

---

_@ntBre reviewed on 2025-02-17 14:16_

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/python_version.rs`:199 on 2025-02-17 14:16_

That totally makes sense.

This ended up also being helped by your suggestion only to convert to `AstPythonVersion` in the `Configuration`. I renamed `serialize` and `deserialize` as suggested but was able to delete the `_option` variants entirely.

We could still use a newtype or just move the conversion functions to the formatter where they're used (and drop the generics). What do you think?

---

_@MichaReiser reviewed on 2025-02-17 14:19_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/python_version.rs`:199 on 2025-02-17 14:19_

Can you point me to where it is used now? 

---

_Comment by @github-actions[bot] on 2025-02-17 14:25_

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

_@ntBre reviewed on 2025-02-17 14:25_

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/python_version.rs`:199 on 2025-02-17 14:25_

Of course, sorry about that. 

This is the only usage of `try_serialize_from` and `deserialize_into`.

https://github.com/astral-sh/ruff/blob/67b2ffc9ffcb8a266fbbf01dbcd7edccfc485a9e/crates/ruff_python_formatter/src/options.rs#L23-L31

---

_@MichaReiser reviewed on 2025-02-17 14:33_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/python_version.rs`:199 on 2025-02-17 14:33_

I would just move it into the formatter. Less feature gating and it is more obvious when it needs to be removed. 

I'm surprised that we don't need it in the linter? 

---

_@ntBre reviewed on 2025-02-17 15:00_

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/python_version.rs`:199 on 2025-02-17 15:00_

Sounds good.

Yeah I was surprised too, but in the linter it was all handled by using the derived (de)serialize implementations for the existing `PythonVersion` in the `Options` and just mapping those to `AstPythonVersion` in the `Configuration`. I guess it's basically acting like the newtype already.

---

_@MichaReiser reviewed on 2025-02-17 15:08_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/python_version.rs`:199 on 2025-02-17 15:08_

I don't think `PyFormatOptions` is used externally (other than tests)? So we could change it to use `PythonVersion`.


The naming might be a bit confusing but the `PyFormatOptions` isn't the same as the other `Options` struct. It is an internal (minus maybe the playground?) options struct that's used by the internal formatter api.

---

_@ntBre reviewed on 2025-02-17 15:41_

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/python_version.rs`:199 on 2025-02-17 15:41_

Oh nice, that let me get rid of the serde stuff entirely. I had to update a bunch of JSON files to use the dotted version syntax, but I guess we don't read files like that from users?

The JSON files were referenced in

https://github.com/astral-sh/ruff/blob/b5cd4f2f70408b8ba2ebd32e554d0fef2472e9c2/crates/ruff_python_formatter/tests/fixtures.rs#L175-L186

and 

https://github.com/astral-sh/ruff/blob/b5cd4f2f70408b8ba2ebd32e554d0fef2472e9c2/crates/ruff_python_formatter/tests/fixtures.rs#L20-L30

I think I was scared of breaking `black_compatibility` in the first draft of this PR, but from reading the docs, I think `black` uses TOML too, so I'm guessing these JSON files are just for our tests like you said.

---

_@ntBre reviewed on 2025-02-17 17:23_

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/python_version.rs`:199 on 2025-02-17 17:23_

I updated the `Display` implementation for the tests and accepted output snap changes for the `format` test here and that also let me delete the `From` and `TryFrom` impls for the formatter. Running tests locally and then I'll push that here too. That should finally be a net decrease in code!

---

_@ntBre reviewed on 2025-02-17 17:45_

---

_Review comment by @ntBre on `crates/ruff_linter/src/settings/types.rs`:49 on 2025-02-17 17:45_

We could almost get rid of this `Default` too so that there would be nothing to keep in sync, but this one is used in `ruff_wasm::Workspace::default_settings`.

---

_Comment by @ntBre on 2025-02-17 17:52_

Thanks @MichaReiser for the very helpful feedback here. I thought it was a lost cause on Friday, but with your help, I think it might be a nice change now.

Apologies for such a large diff. Many of the changes had to happen at the same time, but let me know if there's a way to make it easier to review. I could probably come up with a more useful commit ordering if nothing else.

---

_Marked ready for review by @ntBre on 2025-02-17 17:53_

---

_Review requested from @AlexWaygood by @ntBre on 2025-02-17 17:53_

---

_Renamed from "Try unifying linter and formatter `PythonVersion` enums" to "Use `ast::PythonVersion` internally in the formatter and linter" by @ntBre on 2025-02-17 17:54_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/options.rs`:26 on 2025-02-18 06:48_

nit: I'd use `ast::PythonVersion` instead as I'd find that a useful indicator that it's from the `ruff_python_ast` crate (similar for wherever `AstPythonVersion` is being used

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/options.rs`:481 on 2025-02-18 06:50_

It might be useful to add a comment on top of this variant and the one in the linter to mention the use-case for this which, I think, is mainly for command-line purposes. Or, a docstring but only if that does not leak into the command-line help menu.

---

_@dhruvmanila reviewed on 2025-02-18 06:50_

Thank you for doing this. I've a couple of minor comments but this looks good. I'll let Micha give the green light on this as the main reviewer.

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:732 on 2025-02-18 08:05_

```suggestion
            target_version: self.target_version.into(),
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_annotations/rules/definition.rs`:526 on 2025-02-18 08:07_

Nit: A bit late now but it could have been worth adding a `minor` method to reduce the diff size.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/settings/types.rs`:49 on 2025-02-18 08:11_

Could we create a manual `Default` implementation that calls `PythonVersion` internally (and unwraps)? But we should make sure that there's a doctest or unit test that asserts that `Default::default` can be called successfully

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/python_version.rs`:100 on 2025-02-18 08:14_

What's the reason that this module needs to be `pub`?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/black/cases/funcdef_return_type_trailing_comma.options.json`:1 on 2025-02-18 08:15_

We'll need to update this import script 

https://github.com/astral-sh/ruff/blob/02297d4b7b2ab177bac79ed54920cf7ddcba7012/crates/ruff_python_formatter/resources/test/fixtures/import_black_tests.py#L57-L62



---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/options.rs`:481 on 2025-02-18 08:20_

I don't think this type is used anymore (other than in a public module export). We should remove it.

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/pyproject.rs`:1 on 2025-02-18 08:23_

This seems a no-op change. Revert?

---

_@MichaReiser approved on 2025-02-18 08:23_

Nice. This is great. Finally a step towards fewer PythonVersion structs.

---

_@AlexWaygood reviewed on 2025-02-18 11:12_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:732 on 2025-02-18 11:12_

that doesn't work here because `self.target_version` is an `Option`. But my nit here would be that our usual style in the repo is to fully qualify imports from the `ruff_python_ast` crate rather than aliasing the struct itself, e.g.

```diff
diff --git a/crates/ruff/src/args.rs b/crates/ruff/src/args.rs
index 6d35b226d..6c37948f3 100644
--- a/crates/ruff/src/args.rs
+++ b/crates/ruff/src/args.rs
@@ -21,7 +21,7 @@ use ruff_linter::settings::types::{
     PythonVersion, UnsafeFixes,
 };
 use ruff_linter::{RuleParser, RuleSelector, RuleSelectorParser};
-use ruff_python_ast::python_version::PythonVersion as AstPythonVersion;
+use ruff_python_ast as ast;
 use ruff_source_file::{LineIndex, OneIndexed};
 use ruff_text_size::TextRange;
 use ruff_workspace::configuration::{Configuration, RuleSelection};
@@ -729,7 +729,7 @@ impl CheckCommand {
             preview: resolve_bool_arg(self.preview, self.no_preview).map(PreviewMode::from),
             respect_gitignore: resolve_bool_arg(self.respect_gitignore, self.no_respect_gitignore),
             select: self.select,
-            target_version: self.target_version.map(AstPythonVersion::from),
+            target_version: self.target_version.map(ast::PythonVersion::from),
             unfixable: self.unfixable,
             // TODO(charlie): Included in `pyproject.toml`, but not inherited.
             cache_dir: self.cache_dir,
@@ -771,7 +771,7 @@ impl FormatCommand {
             exclude: self.exclude,
             preview: resolve_bool_arg(self.preview, self.no_preview).map(PreviewMode::from),
             force_exclude: resolve_bool_arg(self.force_exclude, self.no_force_exclude),
-            target_version: self.target_version.map(AstPythonVersion::from),
+            target_version: self.target_version.map(ast::PythonVersion::from),
             cache_dir: self.cache_dir,
             extension: self.extension,
             ..ExplicitConfigOverrides::default()
@@ -801,7 +801,7 @@ impl AnalyzeGraphCommand {
                 None
             },
             preview: resolve_bool_arg(self.preview, self.no_preview).map(PreviewMode::from),
-            target_version: self.target_version.map(AstPythonVersion::from),
+            target_version: self.target_version.map(ast::PythonVersion::from),
             ..ExplicitConfigOverrides::default()
         };
 
@@ -1265,7 +1265,7 @@ struct ExplicitConfigOverrides {
     preview: Option<PreviewMode>,
     respect_gitignore: Option<bool>,
     select: Option<Vec<RuleSelector>>,
-    target_version: Option<AstPythonVersion>,
+    target_version: Option<ast::PythonVersion>,
     unfixable: Option<Vec<RuleSelector>>,
     // TODO(charlie): Captured in pyproject.toml as a default, but not part of `Settings`.
     cache_dir: Option<PathBuf>,
diff --git a/crates/ruff_python_ast/src/lib.rs b/crates/ruff_python_ast/src/lib.rs
index fa3e778fd..0fe6b41e1 100644
--- a/crates/ruff_python_ast/src/lib.rs
+++ b/crates/ruff_python_ast/src/lib.rs
@@ -6,6 +6,7 @@ pub use generated::*;
 pub use int::*;
 pub use nodes::*;
 pub use operator_precedence::*;
+pub use python_version::*;
 
 pub mod comparable;
 pub mod docstrings;
diff --git a/crates/ruff_python_ast/src/python_version.rs b/crates/ruff_python_ast/src/python_version.rs
index a54c8a2b2..cdbb9c8f3 100644
--- a/crates/ruff_python_ast/src/python_version.rs
+++ b/crates/ruff_python_ast/src/python_version.rs
@@ -97,7 +97,7 @@ impl fmt::Display for PythonVersion {
 }
 
 #[cfg(feature = "serde")]
-pub mod serde {
+mod serde {
     use super::PythonVersion;
 
     impl<'de> serde::Deserialize<'de> for PythonVersion {
```

---

_@AlexWaygood reviewed on 2025-02-18 11:27_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/settings/types.rs`:28 on 2025-02-18 11:27_

The `PartialOrd` and `Ord` implementations on this enum are now unused. Conceptually they make sense, but I think I'm inclined to remove them unless they're used.

We might also be able to remove the `CacheKey` implementation? Things seem to compile without it... but I don't know enough about how Ruff's cache works to be confident about that. @MichaReiser might know more

---

_@AlexWaygood reviewed on 2025-02-18 11:28_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/settings/types.rs`:85 on 2025-02-18 11:28_

this method only seems to be used from the `ruff_workspace` crate so it might make sense to move it there

---

_@AlexWaygood reviewed on 2025-02-18 11:30_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:732 on 2025-02-18 11:30_

hehe, my nit is basically the same as @dhruvmanila's in https://github.com/astral-sh/ruff/pull/16170/files#r1959159116 😄

---

_@AlexWaygood approved on 2025-02-18 11:33_

This is fantastic!

---

_@ntBre reviewed on 2025-02-18 13:55_

---

_Review comment by @ntBre on `crates/ruff_linter/src/settings/types.rs`:49 on 2025-02-18 13:55_

Oh that's a great idea, thanks!

---

_@ntBre reviewed on 2025-02-18 14:12_

---

_Review comment by @ntBre on `crates/ruff_linter/src/settings/types.rs`:28 on 2025-02-18 14:12_

I think `CacheKey` is needed for WASM, some of the earlier CI jobs were failing when I tried removing this (or really when I failed to feature gate it properly).

I'll remove the `Ord` impls.

---

_@ntBre reviewed on 2025-02-18 14:15_

---

_Review comment by @ntBre on `crates/ruff_linter/src/settings/types.rs`:28 on 2025-02-18 14:15_

Oh maybe not, I ran clippy with `--target wasm32-unknown-unknown` and it didn't cause any errors. I'll try removing that too!

---

_@ntBre reviewed on 2025-02-18 14:26_

---

_Review comment by @ntBre on `crates/ruff_python_ast/src/python_version.rs`:100 on 2025-02-18 14:26_

It doesn't, good catch. I made it `pub` while moving the `adapter` functions into it but then deleted those.

---

_@ntBre reviewed on 2025-02-18 14:29_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/resources/test/fixtures/black/cases/funcdef_return_type_trailing_comma.options.json`:1 on 2025-02-18 14:29_

I replaced the f-string with just `version.strip()`.

---

_@ntBre reviewed on 2025-02-18 14:30_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/options.rs`:26 on 2025-02-18 14:30_

Good idea, thanks!

---

_@ntBre reviewed on 2025-02-18 14:31_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/src/options.rs`:481 on 2025-02-18 14:31_

Wow, I actually could delete one of the `PythonVersion`s and I didn't even notice... Done!

---

_Review requested from @carljm by @ntBre on 2025-02-18 14:46_

---

_Review requested from @sharkdp by @ntBre on 2025-02-18 14:46_

---

_Comment by @ntBre on 2025-02-18 14:46_

Thank you all for the reviews! I think I took care of all the comments. If you review again, you may want to skip the last commit. I made `ast::python_version` private since Alex suggested `pub use python_version::*` and replaced all of the `ruff_python_ast::python_version` imports with just `ruff_python_ast`. I also merged a couple of separate `ruff_python_ast` imports I noticed while I was doing it.

---

_Comment by @ntBre on 2025-02-18 14:47_

Oh, maybe it won't be the last commit anymore. Let me merge main.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_annotations/rules/definition.rs`:526 on 2025-02-18 15:44_

`type_hint_resolves_to_any()` should probably take a `version: ast::PythonVersion` rather than a `minor_version: u8` argument, looking at this. It would make the method more strongly typed.

<details>
<summary>Patch</summary>

```diff
diff --git a/crates/ruff_linter/src/rules/flake8_annotations/rules/definition.rs b/crates/ruff_linter/src/rules/flake8_annotations/rules/definition.rs
index 692ed478d..e637f0ea1 100644
--- a/crates/ruff_linter/src/rules/flake8_annotations/rules/definition.rs
+++ b/crates/ruff_linter/src/rules/flake8_annotations/rules/definition.rs
@@ -523,7 +523,7 @@ fn check_dynamically_typed<F>(
             if type_hint_resolves_to_any(
                 parsed_annotation.expression(),
                 checker,
-                checker.settings.target_version.minor,
+                checker.settings.target_version,
             ) {
                 diagnostics.push(Diagnostic::new(
                     AnyType { name: func() },
@@ -532,7 +532,7 @@ fn check_dynamically_typed<F>(
             }
         }
     } else {
-        if type_hint_resolves_to_any(annotation, checker, checker.settings.target_version.minor) {
+        if type_hint_resolves_to_any(annotation, checker, checker.settings.target_version) {
             diagnostics.push(Diagnostic::new(
                 AnyType { name: func() },
                 annotation.range(),
diff --git a/crates/ruff_linter/src/rules/ruff/rules/implicit_optional.rs b/crates/ruff_linter/src/rules/ruff/rules/implicit_optional.rs
index 6c10ae503..de90e3b6e 100644
--- a/crates/ruff_linter/src/rules/ruff/rules/implicit_optional.rs
+++ b/crates/ruff_linter/src/rules/ruff/rules/implicit_optional.rs
@@ -177,7 +177,7 @@ pub(crate) fn implicit_optional(checker: &Checker, parameters: &Parameters) {
                 let Some(expr) = type_hint_explicitly_allows_none(
                     parsed_annotation.expression(),
                     checker,
-                    checker.settings.target_version.minor,
+                    checker.settings.target_version,
                 ) else {
                     continue;
                 };
@@ -195,7 +195,7 @@ pub(crate) fn implicit_optional(checker: &Checker, parameters: &Parameters) {
             let Some(expr) = type_hint_explicitly_allows_none(
                 annotation,
                 checker,
-                checker.settings.target_version.minor,
+                checker.settings.target_version,
             ) else {
                 continue;
             };
diff --git a/crates/ruff_linter/src/rules/ruff/typing.rs b/crates/ruff_linter/src/rules/ruff/typing.rs
index 3faea2fa8..a12ab9b6d 100644
--- a/crates/ruff_linter/src/rules/ruff/typing.rs
+++ b/crates/ruff_linter/src/rules/ruff/typing.rs
@@ -10,10 +10,10 @@ use crate::checkers::ast::Checker;
 ///
 /// A known type is either a builtin type, any object from the standard library,
 /// or a type from the `typing_extensions` module.
-fn is_known_type(qualified_name: &QualifiedName, minor_version: u8) -> bool {
+fn is_known_type(qualified_name: &QualifiedName, version: ast::PythonVersion) -> bool {
     match qualified_name.segments() {
         ["" | "typing_extensions", ..] => true,
-        [module, ..] => is_known_standard_library(minor_version, module),
+        [module, ..] => is_known_standard_library(version.minor, module),
         _ => false,
     }
 }
@@ -70,7 +70,11 @@ enum TypingTarget<'a> {
 }
 
 impl<'a> TypingTarget<'a> {
-    fn try_from_expr(expr: &'a Expr, checker: &'a Checker, minor_version: u8) -> Option<Self> {
+    fn try_from_expr(
+        expr: &'a Expr,
+        checker: &'a Checker,
+        version: ast::PythonVersion,
+    ) -> Option<Self> {
         let semantic = checker.semantic();
         match expr {
             Expr::Subscript(ast::ExprSubscript { value, slice, .. }) => {
@@ -91,7 +95,7 @@ impl<'a> TypingTarget<'a> {
                                 resolve_slice_value(slice.as_ref()).next(),
                             ))
                         } else {
-                            if is_known_type(&qualified_name, minor_version) {
+                            if is_known_type(&qualified_name, version) {
                                 Some(TypingTarget::Known)
                             } else {
                                 Some(TypingTarget::Unknown)
@@ -137,7 +141,7 @@ impl<'a> TypingTarget<'a> {
                         )
                     {
                         Some(TypingTarget::Hashable)
-                    } else if !is_known_type(&qualified_name, minor_version) {
+                    } else if !is_known_type(&qualified_name, version) {
                         // If it's not a known type, we assume it's `Any`.
                         Some(TypingTarget::Unknown)
                     } else {
@@ -149,7 +153,7 @@ impl<'a> TypingTarget<'a> {
     }
 
     /// Check if the [`TypingTarget`] explicitly allows `None`.
-    fn contains_none(&self, checker: &Checker, minor_version: u8) -> bool {
+    fn contains_none(&self, checker: &Checker, version: ast::PythonVersion) -> bool {
         match self {
             TypingTarget::None
             | TypingTarget::Optional(_)
@@ -162,10 +166,10 @@ impl<'a> TypingTarget<'a> {
                 resolve_slice_value(slice).any(|element| {
                     // Literal can only contain `None`, a literal value, other `Literal`
                     // or an enum value.
-                    match TypingTarget::try_from_expr(element, checker, minor_version) {
+                    match TypingTarget::try_from_expr(element, checker, version) {
                         None | Some(TypingTarget::None) => true,
                         Some(new_target @ TypingTarget::Literal(_)) => {
-                            new_target.contains_none(checker, minor_version)
+                            new_target.contains_none(checker, version)
                         }
                         _ => false,
                     }
@@ -173,35 +177,32 @@ impl<'a> TypingTarget<'a> {
             }),
             TypingTarget::Union(slice) => slice.is_some_and(|slice| {
                 resolve_slice_value(slice).any(|element| {
-                    TypingTarget::try_from_expr(element, checker, minor_version)
+                    TypingTarget::try_from_expr(element, checker, version)
                         .map_or(true, |new_target| {
-                            new_target.contains_none(checker, minor_version)
+                            new_target.contains_none(checker, version)
                         })
                 })
             }),
             TypingTarget::PEP604Union(left, right) => [left, right].iter().any(|element| {
-                TypingTarget::try_from_expr(element, checker, minor_version)
-                    .map_or(true, |new_target| {
-                        new_target.contains_none(checker, minor_version)
-                    })
+                TypingTarget::try_from_expr(element, checker, version).map_or(true, |new_target| {
+                    new_target.contains_none(checker, version)
+                })
             }),
             TypingTarget::Annotated(expr) => expr.is_some_and(|expr| {
-                TypingTarget::try_from_expr(expr, checker, minor_version)
-                    .map_or(true, |new_target| {
-                        new_target.contains_none(checker, minor_version)
-                    })
+                TypingTarget::try_from_expr(expr, checker, version).map_or(true, |new_target| {
+                    new_target.contains_none(checker, version)
+                })
             }),
             TypingTarget::ForwardReference(expr) => {
-                TypingTarget::try_from_expr(expr, checker, minor_version)
-                    .map_or(true, |new_target| {
-                        new_target.contains_none(checker, minor_version)
-                    })
+                TypingTarget::try_from_expr(expr, checker, version).map_or(true, |new_target| {
+                    new_target.contains_none(checker, version)
+                })
             }
         }
     }
 
     /// Check if the [`TypingTarget`] explicitly allows `Any`.
-    fn contains_any(&self, checker: &Checker, minor_version: u8) -> bool {
+    fn contains_any(&self, checker: &Checker, version: ast::PythonVersion) -> bool {
         match self {
             TypingTarget::Any => true,
             // `Literal` cannot contain `Any` as it's a dynamic value.
@@ -213,31 +214,23 @@ impl<'a> TypingTarget<'a> {
             | TypingTarget::Unknown => false,
             TypingTarget::Union(slice) => slice.is_some_and(|slice| {
                 resolve_slice_value(slice).any(|element| {
-                    TypingTarget::try_from_expr(element, checker, minor_version)
-                        .map_or(true, |new_target| {
-                            new_target.contains_any(checker, minor_version)
-                        })
+                    TypingTarget::try_from_expr(element, checker, version)
+                        .map_or(true, |new_target| new_target.contains_any(checker, version))
                 })
             }),
             TypingTarget::PEP604Union(left, right) => [left, right].iter().any(|element| {
-                TypingTarget::try_from_expr(element, checker, minor_version)
-                    .map_or(true, |new_target| {
-                        new_target.contains_any(checker, minor_version)
-                    })
+                TypingTarget::try_from_expr(element, checker, version)
+                    .map_or(true, |new_target| new_target.contains_any(checker, version))
             }),
             TypingTarget::Annotated(expr) | TypingTarget::Optional(expr) => {
                 expr.is_some_and(|expr| {
-                    TypingTarget::try_from_expr(expr, checker, minor_version)
-                        .map_or(true, |new_target| {
-                            new_target.contains_any(checker, minor_version)
-                        })
+                    TypingTarget::try_from_expr(expr, checker, version)
+                        .map_or(true, |new_target| new_target.contains_any(checker, version))
                 })
             }
             TypingTarget::ForwardReference(expr) => {
-                TypingTarget::try_from_expr(expr, checker, minor_version)
-                    .map_or(true, |new_target| {
-                        new_target.contains_any(checker, minor_version)
-                    })
+                TypingTarget::try_from_expr(expr, checker, version)
+                    .map_or(true, |new_target| new_target.contains_any(checker, version))
             }
         }
     }
@@ -253,9 +246,9 @@ impl<'a> TypingTarget<'a> {
 pub(crate) fn type_hint_explicitly_allows_none<'a>(
     annotation: &'a Expr,
     checker: &'a Checker,
-    minor_version: u8,
+    version: ast::PythonVersion,
 ) -> Option<&'a Expr> {
-    match TypingTarget::try_from_expr(annotation, checker, minor_version) {
+    match TypingTarget::try_from_expr(annotation, checker, version) {
         None |
             // Short circuit on top level `None`, `Any` or `Optional`
             Some(TypingTarget::None | TypingTarget::Optional(_) | TypingTarget::Any) => None,
@@ -264,10 +257,10 @@ pub(crate) fn type_hint_explicitly_allows_none<'a>(
         // is found nested inside another type, then the outer type should
         // be returned.
         Some(TypingTarget::Annotated(expr)) => {
-            expr.and_then(|expr| type_hint_explicitly_allows_none(expr, checker, minor_version))
+            expr.and_then(|expr| type_hint_explicitly_allows_none(expr, checker, version))
         }
         Some(target) => {
-            if target.contains_none(checker, minor_version) {
+            if target.contains_none(checker, version) {
                 return None;
             }
             Some(annotation)
@@ -281,9 +274,9 @@ pub(crate) fn type_hint_explicitly_allows_none<'a>(
 pub(crate) fn type_hint_resolves_to_any(
     annotation: &Expr,
     checker: &Checker,
-    minor_version: u8,
+    version: ast::PythonVersion,
 ) -> bool {
-    match TypingTarget::try_from_expr(annotation, checker, minor_version) {
+    match TypingTarget::try_from_expr(annotation, checker, version) {
         // Short circuit on top level `Any`
         None | Some(TypingTarget::Any) => true,
         // `Optional` is `Optional[Any]` which is `Any | None`.
@@ -291,39 +284,43 @@ pub(crate) fn type_hint_resolves_to_any(
         // Top-level `Annotated` node should check if the inner type resolves
         // to `Any`.
         Some(TypingTarget::Annotated(expr)) => {
-            expr.is_some_and(|expr| type_hint_resolves_to_any(expr, checker, minor_version))
+            expr.is_some_and(|expr| type_hint_resolves_to_any(expr, checker, version))
         }
-        Some(target) => target.contains_any(checker, minor_version),
+        Some(target) => target.contains_any(checker, version),
     }
 }
 
 #[cfg(test)]
 mod tests {
     use super::is_known_type;
+    use ruff_python_ast as ast;
     use ruff_python_ast::name::QualifiedName;
 
     #[test]
     fn test_is_known_type() {
-        assert!(is_known_type(&QualifiedName::builtin("int"), 11));
+        assert!(is_known_type(
+            &QualifiedName::builtin("int"),
+            ast::PythonVersion::PY311
+        ));
         assert!(is_known_type(
             &QualifiedName::from_iter(["builtins", "int"]),
-            11
+            ast::PythonVersion::PY311
         ));
         assert!(is_known_type(
             &QualifiedName::from_iter(["typing", "Optional"]),
-            11
+            ast::PythonVersion::PY311
         ));
         assert!(is_known_type(
             &QualifiedName::from_iter(["typing_extensions", "Literal"]),
-            11
+            ast::PythonVersion::PY311
         ));
         assert!(is_known_type(
             &QualifiedName::from_iter(["zoneinfo", "ZoneInfo"]),
-            11
+            ast::PythonVersion::PY311
         ));
         assert!(!is_known_type(
             &QualifiedName::from_iter(["zoneinfo", "ZoneInfo"]),
-            8
+            ast::PythonVersion::PY38
         ));
     }
 }
```

</details>

Should probably be a followup, though; this is already a big PR and the change isn't directly related

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/settings/types.rs`:41 on 2025-02-18 15:51_

```suggestion
        // SAFETY: the unit test `default_python_version_works()` checks that this doesn't panic
        Self::try_from(ast::PythonVersion::default()).unwrap()
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_formatter/tests/fixtures.rs`:186 on 2025-02-18 15:54_

```suggestion
                .unwrap_or_else(|_| panic!("Expected option file {options_path:?} to be a valid Json file"));
```

---

_@AlexWaygood approved on 2025-02-18 15:55_

---

_@ntBre reviewed on 2025-02-18 15:59_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/fixtures.rs`:186 on 2025-02-18 15:59_

Good catch. I also made this change in the test above where I got my inspiration.

---

_@ntBre reviewed on 2025-02-18 16:37_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_annotations/rules/definition.rs`:526 on 2025-02-18 16:37_

Ooh good idea. I'll save this as a fast follow-up.

---

_Merged by @ntBre on 2025-02-18 17:03_

---

_Closed by @ntBre on 2025-02-18 17:03_

---

_Branch deleted on 2025-02-18 17:03_

---
