```yaml
number: 19475
title: "[ty] Implemented partial support for \"find references\" language server feature."
type: pull_request
state: merged
author: UnboundVariable
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: references
created_at: 2025-07-22T04:52:10Z
updated_at: 2025-07-23T16:16:26Z
url: https://github.com/astral-sh/ruff/pull/19475
synced_at: 2026-01-12T15:56:40Z
```

# [ty] Implemented partial support for "find references" language server feature.

---

_@UnboundVariable_

This PR adds basic support for the "find all references" language server feature. This same functionality will also be used to eventually implement "rename".

This PR searches for references only within the current source file. I decided to post this PR for review before extending it to support multi-file searches. Even without multi-file searches, this feature has utility.

Key question for reviewers: Do you think the approach I've taken is correct, or is there a more optimal approach — for example, one that makes better use of the `UseDef` maps in the semantic index. I don't think the `UseDef` maps store enough details to eliminate the need for an AST walk, but I could be wrong.

My current approach leverages the existing `GotoTarget` logic that is used in a number of the other language server features. While I think there are more optimal ways to implement this, I think they would involve replicating a lot of logic that is already provided by `GotoTarget`.

---

_Review requested from @carljm by @UnboundVariable on 2025-07-22 04:52_

---

_Review requested from @MichaReiser by @UnboundVariable on 2025-07-22 04:52_

---

_Review requested from @AlexWaygood by @UnboundVariable on 2025-07-22 04:52_

---

_Review requested from @sharkdp by @UnboundVariable on 2025-07-22 04:52_

---

_Review requested from @dcreager by @UnboundVariable on 2025-07-22 04:52_

---

_Comment by @github-actions[bot] on 2025-07-22 04:55_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `server` added by @MichaReiser on 2025-07-22 06:29_

---

_Label `ty` added by @MichaReiser on 2025-07-22 06:29_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto.rs`:324 on 2025-07-22 06:34_

Nit: Rename to `to_string`. `as_str` suggests that it returns a `&str` which it clearly doesn't
```suggestion
    pub(crate) fn to_string(&self) -> Option<String> {
```

We also use the following naming convention:

* `as_`: For conversions that take `&self` and are very cheap (don't require allocations)
* `to_`: For conversions that take `&self` but it isn't free (may require allocations)
* `into_`: For conversions that consume `self`

We found this naming scheme useful because it provides callers with the relevant information if calling the same function multiple times or if they should consider caching the result (e.g. calling `to_string` twice for `if goto.to_string() == Some("test") { print!("{:?}", goto.to_string() }` should be avoided, but would be fine if it were `as_str`)

---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto.rs`:324 on 2025-07-22 06:36_

Nit: This is not a hot function, so I'm fine leaving this as is but we could consider returning a `std::cow::Cow<str>` here to avoid allocating in almost all cases (the module one is the only case where we need to return an owned string and even that is only necessary if it contains any spaces)
```suggestion
    pub(crate) fn to_string(&self) -> Option<Cow<str>> {
    	match self {
            GotoTarget::Expression(expression) => match expression {
                ast::ExprRef::Name(name) => Some(name.id.as_str().into()),
                ast::ExprRef::Attribute(attr) => Some(attr.attr.as_str().into()),
                _ => None,
            },
            GotoTarget::FunctionDef(function) => Some(function.name.as_str().into()),
            GotoTarget::ClassDef(class) => Some(class.name.as_str().into()),
            GotoTarget::Parameter(parameter) => Some(parameter.name.as_str().into()),
            GotoTarget::ImportSymbolAlias { alias, .. } => {
```

---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto.rs`:361 on 2025-07-22 06:37_

Nit: You could avoid the need for the closure by using the try operator
```suggestion
                Some(except.name.as_ref()?.as_str().to_string())
```

---

_Review comment by @MichaReiser on `crates/ty_ide/src/references.rs`:16 on 2025-07-22 06:37_

```suggestion
//! This module implements the core functionality of the "references" and
//! "rename" language server features. It locates all references to a named
//! symbol. Unlike a simple text search for the symbol's name, this is
//! a "semantic search" where the text and the semantic meaning must match.
//
//! Some symbols (such as parameters and local variables) are visible only
//! within their scope. All other symbols, such as those defined at the global
//! scope or within classes, are visible outside of the module. Finding
//! all references to these externally-visible symbols therefore requires
//! an expensive search of all source files in the workspace.
```

---

_Review comment by @MichaReiser on `crates/ty_ide/src/references.rs`:190 on 2025-07-22 06:44_

Calling `find_goto_target` here again seems suboptimal because it requires traversing the entire AST again up to `offset`. This seems pretty expensive considering that we repeat it for every name expression. 

I think what we should do here instead is to add a method to construct a goto target from a `CoveringNode` and we could integrate the `CoveringNodeVisitor` into your visitor. 

---

_Review comment by @MichaReiser on `crates/ty_ide/src/references.rs`:214 on 2025-07-22 06:47_

Nit: Move into the same impl block
```suggestion

```

---

_Review comment by @MichaReiser on `crates/ty_ide/src/references.rs`:203 on 2025-07-22 06:50_

Nit: It might make sense to add a contsructor to `NavigationTarget` for the (seemingly very common case) where the focus and full range are identical so that this can be written as `NavigationTarget::new(self.file, range)` 

---

_Review comment by @MichaReiser on `crates/ty_ide/src/references.rs`:244 on 2025-07-22 06:53_

This is more a nit and something that might be worth thinking about going forward. Matching on `NavigationTarget` seems a bit finky. `NavigationTarget` is also somewhat costly to construct because of the inner `Vec` and overall, doesn't seem to be the right abstraction. 

Would it make sense to have a `DefinitionTarget` enum that stands in between `GotoTarget` and `NavigationTargets`?

---

_@MichaReiser reviewed on 2025-07-22 06:53_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-22 11:37_

---

_Review request for @carljm removed by @carljm on 2025-07-22 20:59_

---

_@UnboundVariable reviewed on 2025-07-23 00:19_

---

_Review comment by @UnboundVariable on `crates/ty_ide/src/references.rs`:190 on 2025-07-23 00:19_

This is done only in cases where the name expression text matches that of the target name, so it's perhaps not as expensive as you feared, but it is still somewhat expensive. I'll implement your suggested approach.

---

_@UnboundVariable reviewed on 2025-07-23 00:21_

---

_Review comment by @UnboundVariable on `crates/ty_ide/src/references.rs`:244 on 2025-07-23 00:21_

Yeah, I think that's a reasonable suggestion, but I'd prefer to do that in a separate PR.

---

_Review comment by @UnboundVariable on `crates/ty_ide/src/references.rs`:190 on 2025-07-23 02:09_

I like how this approach turned out!

When implementing this, I found (what I presume is a long-standing) bug in the `ruff_python_ast` crate. The `walk_annotation` function was calling `enter_node` and `leave_node` twice on the top-level node of the annotation expression.

---

_@UnboundVariable reviewed on 2025-07-23 02:09_

---

_Comment by @UnboundVariable on 2025-07-23 02:12_

I've incorporated changes from the code review (thanks!) and added support for multi-file reference searches. That was pretty straightforward, as you predicted.

---

_Comment by @github-actions[bot] on 2025-07-23 02:18_

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

_Review comment by @MichaReiser on `crates/ty_ide/src/references.rs`:30 on 2025-07-23 07:22_

Hmm, it's a bit annoying that we don't have access to the `ProjectDatabase` here. I went ahead and created a PR that inverts the dependency between `ty_project` and `ty_ide` so that this function can call `db.project().files(db)` directly (https://github.com/astral-sh/ruff/pull/19501)


This also removes the need for collecting all the files in the test setup because the tests then also use the project's file discovery to find the references in the project:

```patch
Subject: [PATCH] [ty] Use `ThinVec` in various places
---
Index: crates/ty_ide/src/lib.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ty_ide/src/lib.rs b/crates/ty_ide/src/lib.rs
--- a/crates/ty_ide/src/lib.rs	(revision 4bf83de1624ec9068260cdf8b032a06a8cc7ef01)
+++ b/crates/ty_ide/src/lib.rs	(date 1753255216582)
@@ -243,7 +243,6 @@
     pub(super) struct CursorTest {
         pub(super) db: ty_project::TestDb,
         pub(super) cursor: Cursor,
-        pub(super) files: Vec<File>,
         _insta_settings_guard: SettingsBindDropGuard,
     }
 
@@ -303,7 +302,6 @@
             ));
 
             let mut cursor: Option<Cursor> = None;
-            let mut files = Vec::new();
 
             for &Source {
                 ref path,
@@ -315,7 +313,6 @@
                     .expect("write to memory file system to be successful");
 
                 let file = system_path_to_file(&db, path).expect("newly written file to existing");
-                files.push(file);
 
                 if let Some(offset) = cursor_offset {
                     // This assert should generally never trip, since
@@ -352,7 +349,6 @@
             CursorTest {
                 db,
                 cursor: cursor.expect("at least one source to contain `<CURSOR>`"),
-                files,
                 _insta_settings_guard: insta_settings_guard,
             }
         }
Index: crates/ty_ide/src/references.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ty_ide/src/references.rs b/crates/ty_ide/src/references.rs
--- a/crates/ty_ide/src/references.rs	(revision 4bf83de1624ec9068260cdf8b032a06a8cc7ef01)
+++ b/crates/ty_ide/src/references.rs	(date 1753255209500)
@@ -27,7 +27,6 @@
     file: File,
     offset: TextSize,
     include_declaration: bool,
-    project_files: impl IntoIterator<Item = File>,
 ) -> Option<Vec<RangedValue<NavigationTargets>>> {
     let parsed = ruff_db::parsed::parsed_module(db, file);
     let module = parsed.load(db);
@@ -53,7 +52,7 @@
     // Check if the symbol is potentially visible outside of this module
     if is_symbol_externally_visible(&goto_target) {
         // Look for references in all other files within the workspace
-        for other_file in project_files {
+        for other_file in &db.project().files(db) {
             // Skip the current file as we already processed it
             if other_file == file {
                 continue;
@@ -281,38 +280,9 @@
 
     impl CursorTest {
         fn references(&self) -> String {
-            let Some(reference_results) = references(
-                &self.db,
-                self.cursor.file,
-                self.cursor.offset,
-                true,
-                std::iter::empty(),
-            ) else {
-                return "No references found".to_string();
-            };
-
-            if reference_results.is_empty() {
-                return "No references found".to_string();
-            }
-
-            self.render_diagnostics(reference_results.into_iter().enumerate().map(
-                |(i, ref_item)| -> ReferenceResult {
-                    ReferenceResult {
-                        index: i,
-                        file_range: ref_item.range,
-                    }
-                },
-            ))
-        }
-
-        fn references_with_project_files(&self, project_files: Vec<File>) -> String {
-            let Some(reference_results) = references(
-                &self.db,
-                self.cursor.file,
-                self.cursor.offset,
-                true,
-                project_files,
-            ) else {
+            let Some(reference_results) =
+                references(&self.db, self.cursor.file, self.cursor.offset, true)
+            else {
                 return "No references found".to_string();
             };
 
@@ -990,7 +960,7 @@
             )
             .build();
 
-        assert_snapshot!(test.references_with_project_files(test.files.clone()), @r"
+        assert_snapshot!(test.references(), @r"
         info[references]: Reference 1
          --> utils.py:2:5
           |
@@ -1075,7 +1045,7 @@
             )
             .build();
 
-        assert_snapshot!(test.references_with_project_files(test.files.clone()), @r"
+        assert_snapshot!(test.references(), @r"
         info[references]: Reference 1
          --> models.py:3:5
           |

```

The other advantage is that we don't need to load the project files if the symbol isn't publicly visible (we can defer query the project files to exactly when they're needed)

---

_Review comment by @MichaReiser on `crates/ty_ide/src/references.rs`:126 on 2025-07-23 07:25_

Nit: I would suggest using an exhaustive match here. It makes it easier for the next person adding a variant to `GotoTarget` to know that they need to think about this.

---

_Review comment by @MichaReiser on `crates/ty_ide/src/references.rs`:219 on 2025-07-23 07:30_

This is a bit a micro optimization but I use this opportunity to show you a rust pattern. 

We have to clone the `ancestors` vector here because `from_ancestors` takes an owned `Vec`. We could avoid the cloning here if we restore the `ancestors` after calling `check_reference_from_covering_node`:

```rust
	let mut ancestors_with_identifier = std::mem::take(&mut self.ancestors); // take the ancestors, that leaves `self.ancestors` empty
	ancestors_with_identifier.push(AnyNodeRef::from(identifier));
  let covering_node = CoveringNode::from_ancestors(ancestors_with_identifier);
	self.check_reference_from_covering_node(&covering_node);

	// now, restore ancestors
	let mut ancestors_with_identifier = covering_node.into_ancestors(); 
	ancestors_with_identifier.pop(); // remove the identifier
	self.ancestors = ancestors_with_identifier;
```


where `CoveringNode::into_ancestors` is

```rust
fn into_ancestors(self) -> Vec<AnyNodeRef> {
	self.ancestors
}
```

---

_Review comment by @MichaReiser on `crates/ty_ide/src/references.rs`:234 on 2025-07-23 07:30_

```suggestion
        let offset = covering_node.node().start();
```

---

_@MichaReiser approved on 2025-07-23 07:33_

---

_Merged by @UnboundVariable on 2025-07-23 16:16_

---

_Closed by @UnboundVariable on 2025-07-23 16:16_

---

_Branch deleted on 2025-07-23 16:16_

---
