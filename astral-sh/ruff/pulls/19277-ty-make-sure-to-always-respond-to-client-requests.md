```yaml
number: 19277
title: "[ty] Make sure to always respond to client requests"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - server
  - ty
assignees: []
merged: true
base: main
head: dhruv/document-snapshot
created_at: 2025-07-11T10:54:21Z
updated_at: 2025-07-11T14:34:10Z
url: https://github.com/astral-sh/ruff/pull/19277
synced_at: 2026-01-10T18:33:12Z
```

# [ty] Make sure to always respond to client requests

---

_Pull request opened by @dhruvmanila on 2025-07-11 10:54_

## Summary

This PR fixes a bug that didn't return a response to the client if the document snapshotting failed.

This is resolved by making sure that the server always creates the document snapshot and embed the any failures inside the snapshot.

Closes: astral-sh/ty#798

## Test Plan

Using the test case as described in the linked issue:


https://github.com/user-attachments/assets/f32833f8-03e5-4641-8c7f-2a536fe2e270




---

_Review requested from @carljm by @dhruvmanila on 2025-07-11 10:54_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-07-11 10:54_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-07-11 10:54_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-07-11 10:54_

---

_Review requested from @dcreager by @dhruvmanila on 2025-07-11 10:54_

---

_Label `bug` added by @dhruvmanila on 2025-07-11 10:54_

---

_Label `server` added by @dhruvmanila on 2025-07-11 10:54_

---

_Label `ty` added by @dhruvmanila on 2025-07-11 10:54_

---

_Comment by @github-actions[bot] on 2025-07-11 10:58_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-11 11:00_

---

_Comment by @github-actions[bot] on 2025-07-11 11:06_

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

_Review comment by @MichaReiser on `crates/ty_server/src/session/index.rs`:147 on 2025-07-11 11:14_

I think we can simplify this by changing `DocumentQuery` to store an `AnySystemPath` (file_path) instead of `file_url`. I quickly tried this locally and it seems we don't really need the URL anywhere and a path is fine. This allows us to remove this `Err` branch entirely and hopefully simplifies things overall.


```patch
Subject: [PATCH] Rename `knot_benchmark` to `ty_benchmark`
---
Index: crates/ty_server/src/system.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ty_server/src/system.rs b/crates/ty_server/src/system.rs
--- a/crates/ty_server/src/system.rs	(revision 08c2c695a2006cf2d141478a76822f0ce755fc0d)
+++ b/crates/ty_server/src/system.rs	(date 1752232570580)
@@ -46,7 +46,7 @@
 
 /// Represents either a [`SystemPath`] or a [`SystemVirtualPath`].
 #[derive(Clone, Debug, Hash, PartialEq, Eq)]
-pub(crate) enum AnySystemPath {
+pub enum AnySystemPath {
     System(SystemPathBuf),
     SystemVirtual(SystemVirtualPathBuf),
 }
Index: crates/ty_server/src/session/index.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ty_server/src/session/index.rs b/crates/ty_server/src/session/index.rs
--- a/crates/ty_server/src/session/index.rs	(revision 08c2c695a2006cf2d141478a76822f0ce755fc0d)
+++ b/crates/ty_server/src/session/index.rs	(date 1752232576248)
@@ -142,18 +142,8 @@
             DocumentKey::NotebookCell {
                 cell_url,
                 notebook_path,
-            } => {
-                let Some(notebook_url) = notebook_path.to_url() else {
-                    return Err(DocumentQueryError::InvalidSystemPath(notebook_path));
-                };
-                (Some(cell_url), notebook_url)
-            }
-            DocumentKey::Notebook(path) | DocumentKey::Text(path) => {
-                let Some(file_url) = path.to_url() else {
-                    return Err(DocumentQueryError::InvalidSystemPath(path));
-                };
-                (None, file_url)
-            }
+            } => (Some(cell_url), notebook_path),
+            DocumentKey::Notebook(path) | DocumentKey::Text(path) => (None, path),
         };
         Ok(controller.make_ref(cell_url, file_url))
     }
@@ -230,15 +220,15 @@
         Self::Notebook(Arc::new(document))
     }
 
-    fn make_ref(&self, cell_url: Option<Url>, file_url: Url) -> DocumentQuery {
+    fn make_ref(&self, cell_url: Option<Url>, file_path: AnySystemPath) -> DocumentQuery {
         match &self {
             Self::Notebook(notebook) => DocumentQuery::Notebook {
                 cell_url,
-                file_url,
+                file_path,
                 notebook: notebook.clone(),
             },
             Self::Text(document) => DocumentQuery::Text {
-                file_url,
+                file_path,
                 document: document.clone(),
             },
         }
@@ -279,14 +269,14 @@
 #[derive(Debug, Clone)]
 pub enum DocumentQuery {
     Text {
-        file_url: Url,
+        file_path: AnySystemPath,
         document: Arc<TextDocument>,
     },
     Notebook {
         /// The selected notebook cell, if it exists.
         cell_url: Option<Url>,
         /// The URL of the notebook.
-        file_url: Url,
+        file_path: AnySystemPath,
         notebook: Arc<NotebookDocument>,
     },
 }
@@ -309,9 +299,9 @@
     }
 
     /// Get the URL for the document selected by this query.
-    pub(crate) fn file_url(&self) -> &Url {
+    pub(crate) fn file_path(&self) -> &AnySystemPath {
         match self {
-            Self::Text { file_url, .. } | Self::Notebook { file_url, .. } => file_url,
+            Self::Text { file_path, .. } | Self::Notebook { file_path, .. } => file_path,
         }
     }
 
@@ -332,7 +322,7 @@
     }
 
     pub(crate) fn file(&self, db: &dyn Db) -> Option<File> {
-        match AnySystemPath::try_from_url(self.file_url()).ok()? {
+        match self.file_path() {
             AnySystemPath::System(path) => system_path_to_file(db, path).ok(),
             AnySystemPath::SystemVirtual(virtual_path) => db
                 .files()
@@ -348,6 +338,4 @@
     InvalidUrl(Url),
     #[error("document not found for key: {0}")]
     NotFound(DocumentKey),
-    #[error("invalid system path: {0}")]
-    InvalidSystemPath(AnySystemPath),
 }
Index: crates/ty_server/src/server/api/diagnostics.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ty_server/src/server/api/diagnostics.rs b/crates/ty_server/src/server/api/diagnostics.rs
--- a/crates/ty_server/src/server/api/diagnostics.rs	(revision 08c2c695a2006cf2d141478a76822f0ce755fc0d)
+++ b/crates/ty_server/src/server/api/diagnostics.rs	(date 1752232433054)
@@ -122,7 +122,7 @@
     };
 
     let Some(file) = document.file(db) else {
-        tracing::info!("No file found for snapshot for `{}`", document.file_url());
+        tracing::info!("No file found for snapshot for `{}`", document.file_path());
         return None;
     };
 
Index: crates/ty_server/src/session.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ty_server/src/session.rs b/crates/ty_server/src/session.rs
--- a/crates/ty_server/src/session.rs	(revision 08c2c695a2006cf2d141478a76822f0ce755fc0d)
+++ b/crates/ty_server/src/session.rs	(date 1752232547360)
@@ -471,14 +471,14 @@
         };
         document
             .file(db)
-            .ok_or_else(|| FileLookupError::NotFound(document.file_url().clone()))
+            .ok_or_else(|| FileLookupError::NotFound(document.file_path().clone()))
     }
 }
 
 #[derive(Debug, thiserror::Error)]
 pub(crate) enum FileLookupError {
-    #[error("file not found for url `{0}`")]
-    NotFound(Url),
+    #[error("file not found for path `{0}`")]
+    NotFound(AnySystemPath),
     #[error(transparent)]
     DocumentQuery(DocumentQueryError),
 }
```

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api.rs`:222 on 2025-07-11 11:19_


```suggestion
            tracing::warn!("Ignoring request id={id} method={} because the URL `{url}` isn't a valid path", R::METHOD);
```

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/diagnostics.rs`:87 on 2025-07-11 11:22_

I suggest passing the document in addition to the snapshot so that `compute_diagnostic` doesn't have to repeat the `document` dance

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/signature_help.rs`:41 on 2025-07-11 11:25_

I don't have a good sense for it but it would be nice if getting the file wouldn't require as much boilerplate. Should we add a `file_with_tracing` (`file_ok`) method that returns an `Option` and has the tracing call internally?

---

_@MichaReiser approved on 2025-07-11 11:26_

Nice find and thanks for fixing this. 

---

_@dhruvmanila reviewed on 2025-07-11 14:16_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/diagnostics.rs`:87 on 2025-07-11 14:16_

This function is used in https://github.com/astral-sh/ruff/blob/08c2c695a2006cf2d141478a76822f0ce755fc0d/crates/ty_server/src/server/api/requests/diagnostic.rs#L42-L42 as well which would mean that the `snapshot > document > file` would need to be done somewhere - either in the document diagnostic handler or inside compute diagnostic.

---

_@dhruvmanila reviewed on 2025-07-11 14:17_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/signature_help.rs`:41 on 2025-07-11 14:17_

Yeah, `file_ok` sounds good

---

_Merged by @dhruvmanila on 2025-07-11 14:27_

---

_Closed by @dhruvmanila on 2025-07-11 14:27_

---

_Branch deleted on 2025-07-11 14:27_

---
