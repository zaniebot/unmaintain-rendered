```yaml
number: 1573
title: Remove need for vendored format/cformat code
type: pull_request
state: merged
author: olliemath
labels: []
assignees: []
merged: true
base: main
head: main
created_at: 2023-01-02T23:30:24Z
updated_at: 2023-01-03T01:33:46Z
url: https://github.com/astral-sh/ruff/pull/1573
synced_at: 2026-01-12T05:36:31Z
```

# Remove need for vendored format/cformat code

---

_Pull request opened by @olliemath on 2023-01-02 23:30_

Thanks to @youknowone we can now remove this vendored code and use the `rustpython_common` version. See also https://github.com/charliermarsh/ruff/issues/1115

---

_@olliemath reviewed on 2023-01-02 23:59_

---

_Review comment by @olliemath on `src/pyflakes/cformat.rs`:38 on 2023-01-02 23:59_

Couldn't quite figure out how to get rid of the need for this clone - inserting an `&k` results in a `returns a value referencing data owned by the current function` error at compile time

---

_Review comment by @charliermarsh on `src/pyflakes/cformat.rs`:38 on 2023-01-03 00:27_

```diff
diff --git a/src/checkers/ast.rs b/src/checkers/ast.rs
index ef4d2f7d..37712c04 100644
--- a/src/checkers/ast.rs
+++ b/src/checkers/ast.rs
@@ -1,13 +1,14 @@
 //! Lint rules based on AST traversal.
 
 use std::path::Path;
+use std::str::FromStr;
 
 use itertools::Itertools;
 use log::error;
 use nohash_hasher::IntMap;
 use rustc_hash::{FxHashMap, FxHashSet};
 use rustpython_ast::{Located, Location};
-use rustpython_common::cformat::{CFormatError, CFormatErrorType};
+use rustpython_common::cformat::{CFormatError, CFormatErrorType, CFormatString};
 use rustpython_parser::ast::{
     Arg, Arguments, Constant, Excepthandler, ExcepthandlerKind, Expr, ExprContext, ExprKind,
     KeywordData, Operator, Stmt, StmtKind, Suite,
@@ -27,6 +28,7 @@ use crate::ast::{branch_detection, cast, helpers, operations, visitor};
 use crate::checks::{Check, CheckCode, CheckKind, DeferralKeyword};
 use crate::docstrings::definition::{Definition, DefinitionKind, Docstring, Documentable};
 use crate::noqa::Directive;
+use crate::pyflakes::cformat::from_parts;
 use crate::python::builtins::{BUILTINS, MAGIC_GLOBALS};
 use crate::python::future::ALL_FEATURE_NAMES;
 use crate::python::typing;
@@ -2325,7 +2327,9 @@ where
                         || self.settings.enabled.contains(&CheckCode::F509)
                     {
                         let location = Range::from_located(expr);
-                        match pyflakes::cformat::CFormatSummary::try_from(value.as_ref()) {
+
+                        let format_string = CFormatString::from_str(&value).unwrap();
+                        match from_parts(format_string.iter()) {
                             Err(CFormatError {
                                 typ: CFormatErrorType::UnsupportedFormatChar(c),
                                 ..
diff --git a/src/pyflakes/cformat.rs b/src/pyflakes/cformat.rs
index cfbf0195..470e2a7e 100644
--- a/src/pyflakes/cformat.rs
+++ b/src/pyflakes/cformat.rs
@@ -7,24 +7,21 @@ use rustpython_common::cformat::{
     CFormatError, CFormatPart, CFormatQuantity, CFormatSpec, CFormatString,
 };
 
-pub(crate) struct CFormatSummary {
+pub(crate) struct CFormatSummary<'a> {
     pub starred: bool,
     pub num_positional: usize,
-    pub keywords: FxHashSet<String>,
+    pub keywords: FxHashSet<&'a str>,
 }
 
-impl TryFrom<&str> for CFormatSummary {
-    type Error = CFormatError;
+pub(crate) fn from_parts<'a>(
+    format_parts: impl Iterator<Item = &'a (usize, CFormatPart<String>)>,
+) -> Result<CFormatSummary<'a>, CFormatError> {
+    let mut starred = false;
+    let mut num_positional = 0;
+    let mut keywords = FxHashSet::default();
 
-    fn try_from(literal: &str) -> Result<Self, Self::Error> {
-        let format_string = CFormatString::from_str(literal)?;
-
-        let mut starred = false;
-        let mut num_positional = 0;
-        let mut keywords = FxHashSet::default();
-
-        for format_part in format_string.iter() {
-            let CFormatPart::Spec(CFormatSpec {
+    for format_part in format_parts {
+        let CFormatPart::Spec(CFormatSpec {
                 ref mapping_key,
                 ref min_field_width,
                 ref precision,
@@ -33,30 +30,29 @@ impl TryFrom<&str> for CFormatSummary {
             {
                 continue;
             };
-            match mapping_key {
-                Some(k) => {
-                    keywords.insert(k.clone());
-                }
-                None => {
-                    num_positional += 1;
-                }
-            };
-            if min_field_width == &Some(CFormatQuantity::FromValuesTuple) {
-                num_positional += 1;
-                starred = true;
+        match mapping_key {
+            Some(k) => {
+                keywords.insert(k.as_str());
             }
-            if precision == &Some(CFormatQuantity::FromValuesTuple) {
+            None => {
                 num_positional += 1;
-                starred = true;
             }
+        };
+        if min_field_width == &Some(CFormatQuantity::FromValuesTuple) {
+            num_positional += 1;
+            starred = true;
+        }
+        if precision == &Some(CFormatQuantity::FromValuesTuple) {
+            num_positional += 1;
+            starred = true;
         }
-
-        Ok(CFormatSummary {
-            starred,
-            num_positional,
-            keywords,
-        })
     }
+
+    Ok(CFormatSummary {
+        starred,
+        num_positional,
+        keywords,
+    })
 }
 
 #[cfg(test)]
diff --git a/src/pyflakes/plugins/strings.rs b/src/pyflakes/plugins/strings.rs
index 0a4036e6..d96b51c2 100644
--- a/src/pyflakes/plugins/strings.rs
+++ b/src/pyflakes/plugins/strings.rs
@@ -94,7 +94,7 @@ pub(crate) fn percent_format_extra_named_arguments(
                 value: Constant::Str(value),
                 ..
             } => {
-                if summary.keywords.contains(value) {
+                if summary.keywords.contains(value.as_str()) {
                     None
                 } else {
                     Some(value.as_str())
@@ -146,7 +146,7 @@ pub(crate) fn percent_format_missing_arguments(
                     value: Constant::Str(value),
                     ..
                 } => {
-                    keywords.insert(value);
+                    keywords.insert(value.as_str());
                 }
                 _ => {
                     return; // Dynamic keys present
@@ -154,16 +154,19 @@ pub(crate) fn percent_format_missing_arguments(
             }
         }
 
-        let missing: Vec<&String> = summary
+        let missing: Vec<&&str> = summary
             .keywords
             .iter()
-            .filter(|k| !keywords.contains(k))
+            .filter(|keyword| !keywords.contains(*keyword))
             .collect();
 
         if !missing.is_empty() {
             checker.add_check(Check::new(
                 CheckKind::PercentFormatMissingArgument(
-                    missing.iter().map(|&s| s.clone()).collect(),
+                    missing
+                        .into_iter()
+                        .map(|keyword| keyword.to_string())
+                        .collect(),
                 ),
                 location,
             ));
```

---

_@charliermarsh reviewed on 2023-01-03 00:27_

---

_Review comment by @charliermarsh on `src/pyflakes/cformat.rs`:38 on 2023-01-03 00:27_

I got this working but it doesn't implement `TryFrom` :/

---

_@charliermarsh reviewed on 2023-01-03 00:27_

---

_Merged by @charliermarsh on 2023-01-03 00:37_

---

_Closed by @charliermarsh on 2023-01-03 00:37_

---

_@olliemath reviewed on 2023-01-03 01:08_

---

_Review comment by @olliemath on `src/pyflakes/cformat.rs`:38 on 2023-01-03 01:08_

Oh nice - lifting the instantiation up one level is a good idea A shame about `TryFrom`, but I guess that was mostly useful when writing unit tests.

---

_@charliermarsh reviewed on 2023-01-03 01:33_

---

_Review comment by @charliermarsh on `src/pyflakes/cformat.rs`:38 on 2023-01-03 01:33_

I just went ahead with your implementation :) Maybe I'll change it later.

---
