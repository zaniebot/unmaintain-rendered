```yaml
number: 16101
title: "add diagnostic `Span` (couples `File` and `TextRange`)"
type: pull_request
state: merged
author: BurntSushi
labels:
  - ty
assignees: []
merged: true
base: main
head: ag/couple-file-with-range
created_at: 2025-02-11T18:02:31Z
updated_at: 2025-02-11T19:55:15Z
url: https://github.com/astral-sh/ruff/pull/16101
synced_at: 2026-01-12T15:55:53Z
```

# add diagnostic `Span` (couples `File` and `TextRange`)

---

_@BurntSushi_

This essentially makes it impossible to construct a `Diagnostic`
that has a `TextRange` but no `File`.

This is meant to be a precursor to multi-span support.

(Note that I consider this more of a prototyping-change and not
necessarily what this is going to look like longer term.)

Reviewers can probably review this PR as one big diff instead of
commit-by-commit.


---

_Review requested from @carljm by @BurntSushi on 2025-02-11 18:02_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-02-11 18:02_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-02-11 18:02_

---

_Review requested from @sharkdp by @BurntSushi on 2025-02-11 18:02_

---

_Comment by @github-actions[bot] on 2025-02-11 18:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @MichaReiser on 2025-02-11 18:10_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/metadata/options.rs`:385 on 2025-02-11 18:12_

Could we directly store the `Span` on `OptionDiagnostic` instead of constructing it here on the fly?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:805 on 2025-02-11 18:12_

Same: I'm sort of inclined to simply store the `Span` directly on the `Diagnostic` struct. But I guess this way is fine too

---

_@MichaReiser approved on 2025-02-11 18:14_

Nice: The only thing that's unclear to me is whether we want to store `Span`s directly on the concrete `Diagnostic` types or if we want to construct the span ad-hoc in the `span` method. I think I'm fine with either. 

---

_@BurntSushi reviewed on 2025-02-11 18:38_

---

_Review comment by @BurntSushi on `crates/red_knot_project/src/metadata/options.rs`:385 on 2025-02-11 18:38_

This is done.

---

_@BurntSushi reviewed on 2025-02-11 18:40_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/src/types/diagnostic.rs`:805 on 2025-02-11 18:40_

This one is a little trickier because a `TypedDiagnostic` always has a `File` and a `Range`, where as a `Span` always has a `File` and an optional `Range`. So I'm going to leave this as-is for now.

---

_@AlexWaygood approved on 2025-02-11 18:47_

Since `Span` is `Copy`, you could make the `range` and `file` methods take `self` rather than `&self`, which would allow you to get rid of a few closures:

<details>
<summary>Patch</summary>

```diff
diff --git a/crates/red_knot_project/src/lib.rs b/crates/red_knot_project/src/lib.rs
index 2126f36a8..bd3dfe60a 100644
--- a/crates/red_knot_project/src/lib.rs
+++ b/crates/red_knot_project/src/lib.rs
@@ -347,7 +347,7 @@ fn check_file_impl(db: &dyn Db, file: File) -> Vec<Box<dyn Diagnostic>> {
     diagnostics.sort_unstable_by_key(|diagnostic| {
         diagnostic
             .span()
-            .and_then(|span| span.range())
+            .and_then(Span::range)
             .unwrap_or_default()
             .start()
     });
diff --git a/crates/red_knot_test/src/diagnostic.rs b/crates/red_knot_test/src/diagnostic.rs
index 56ee51223..7cf258f11 100644
--- a/crates/red_knot_test/src/diagnostic.rs
+++ b/crates/red_knot_test/src/diagnostic.rs
@@ -2,7 +2,7 @@
 //!
 //! We don't assume that we will get the diagnostics in source order.
 
-use ruff_db::diagnostic::Diagnostic;
+use ruff_db::diagnostic::{Diagnostic, Span};
 use ruff_source_file::{LineIndex, OneIndexed};
 use std::ops::{Deref, Range};
 
@@ -27,7 +27,7 @@ where
             .map(|diagnostic| DiagnosticWithLine {
                 line_number: diagnostic
                     .span()
-                    .and_then(|span| span.range())
+                    .and_then(Span::range)
                     .map_or(OneIndexed::from_zero_indexed(0), |range| {
                         line_index.line_index(range.start())
                     }),
diff --git a/crates/red_knot_test/src/matcher.rs b/crates/red_knot_test/src/matcher.rs
index d350ec7c6..d90bcef03 100644
--- a/crates/red_knot_test/src/matcher.rs
+++ b/crates/red_knot_test/src/matcher.rs
@@ -4,7 +4,7 @@ use crate::assertion::{Assertion, ErrorAssertion, InlineFileAssertions};
 use crate::db::Db;
 use crate::diagnostic::SortedDiagnostics;
 use colored::Colorize;
-use ruff_db::diagnostic::{Diagnostic, DiagnosticAsStrError, DiagnosticId};
+use ruff_db::diagnostic::{Diagnostic, DiagnosticAsStrError, DiagnosticId, Span};
 use ruff_db::files::File;
 use ruff_db::source::{line_index, source_text, SourceText};
 use ruff_source_file::{LineIndex, OneIndexed};
@@ -258,7 +258,7 @@ impl Matcher {
     fn column<T: Diagnostic>(&self, diagnostic: &T) -> OneIndexed {
         diagnostic
             .span()
-            .and_then(|span| span.range())
+            .and_then(Span::range)
             .map(|range| {
                 self.line_index
                     .source_location(range.start(), &self.source)
diff --git a/crates/ruff_benchmark/benches/red_knot.rs b/crates/ruff_benchmark/benches/red_knot.rs
index 87366b04d..423446dde 100644
--- a/crates/ruff_benchmark/benches/red_knot.rs
+++ b/crates/ruff_benchmark/benches/red_knot.rs
@@ -11,7 +11,7 @@ use red_knot_project::{Db, ProjectDatabase, ProjectMetadata};
 use red_knot_python_semantic::PythonVersion;
 use ruff_benchmark::criterion::{criterion_group, criterion_main, BatchSize, Criterion};
 use ruff_benchmark::TestFile;
-use ruff_db::diagnostic::{Diagnostic, DiagnosticId, Severity};
+use ruff_db::diagnostic::{Diagnostic, DiagnosticId, Severity, Span};
 use ruff_db::files::{system_path_to_file, File};
 use ruff_db::source::source_text;
 use ruff_db::system::{MemoryFileSystem, SystemPath, SystemPathBuf, TestSystem};
@@ -231,11 +231,11 @@ fn assert_diagnostics(db: &dyn Db, diagnostics: &[Box<dyn Diagnostic>]) {
                 diagnostic.id(),
                 diagnostic
                     .span()
-                    .map(|span| span.file())
+                    .map(Span::file)
                     .map(|file| file.path(db).as_str()),
                 diagnostic
                     .span()
-                    .and_then(|span| span.range())
+                    .and_then(Span::range)
                     .map(Range::<usize>::from),
                 diagnostic.message(),
                 diagnostic.severity(),
diff --git a/crates/ruff_db/src/diagnostic.rs b/crates/ruff_db/src/diagnostic.rs
index 02c701267..5bcec79c5 100644
--- a/crates/ruff_db/src/diagnostic.rs
+++ b/crates/ruff_db/src/diagnostic.rs
@@ -196,7 +196,7 @@ pub struct Span {
 
 impl Span {
     /// Returns the `File` attached to this `Span`.
-    pub fn file(&self) -> File {
+    pub fn file(self) -> File {
         self.file
     }
 
@@ -205,7 +205,7 @@ impl Span {
     /// When there is no range, it is convention to assume that this `Span`
     /// refers to the corresponding `File` as a whole. In some cases, consumers
     /// of this API may use the range `0..0` to represent this case.
-    pub fn range(&self) -> Option<TextRange> {
+    pub fn range(self) -> Option<TextRange> {
         self.range
     }
```

</diff>

(No strong opinion on whether that's worth it, though)

---

_Comment by @MichaReiser on 2025-02-11 18:51_

It's unclear if `Span` can remain `Copy` if we integrate it into Ruff where we don't have a `Copy` identifier for a file.

---

_Comment by @BurntSushi on 2025-02-11 19:41_

Yeah `Span` being `Copy` is interesting. I wasn't thinking too carefully about that yet, but I can see it being annoying if it can't be `Copy`. But I wouldn't be surprised if the refactor in Ruff to permit it was brutal.

I'm going to remove the `Copy` impl, but retain APIs _as if_ it were `Copy` for now. (Which essentially means writing `span.clone()` in some cases.) If we really commit to non-`Copy`, then we'll want to use `&Span` in more places, which is pretty annoying.

---

_Comment by @MichaReiser on 2025-02-11 19:44_

I could see us interning files in Ruff when we create the diagnostics, but yeah, it's somewhat unclear if that's hard. But definitely annoying if `Span` can't be `Copy`.

---

_Merged by @BurntSushi on 2025-02-11 19:55_

---

_Closed by @BurntSushi on 2025-02-11 19:55_

---

_Branch deleted on 2025-02-11 19:55_

---
