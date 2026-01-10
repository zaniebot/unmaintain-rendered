```yaml
number: 12199
title: "Formatter: inconsistent amount of added empty lines around an inner function"
type: issue
state: closed
author: flying-sheep
labels:
  - bug
  - breaking
  - formatter
assignees: []
created_at: 2024-07-05T08:14:20Z
updated_at: 2024-07-20T10:09:57Z
url: https://github.com/astral-sh/ruff/issues/12199
synced_at: 2026-01-10T11:09:54Z
```

# Formatter: inconsistent amount of added empty lines around an inner function

---

_Issue opened by @flying-sheep on 2024-07-05 08:14_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Input:

```py
    if header[0] == "Package":
        def strengthen(k: str) -> str:
            return f"<strong>{k}</strong>"
    else:
        pass
```

Ruff’s output:

```py
    if header[0] == "Package":

        def strengthen(k: str) -> str:
            return f"<strong>{k}</strong>"
    else:
        pass
```

Black’s output:

```py
    if header[0] == "Package":

        def strengthen(k: str) -> str:
            return f"<strong>{k}</strong>"

    else:
        pass
```

I would personally prefer no empty lines to be inserted here, but matching Black’s behavior at least creates a consistent result.

---

_Label `formatter` added by @MichaReiser on 2024-07-05 08:49_

---

_Comment by @MichaReiser on 2024-07-05 08:50_

My nemesis, Black's empty line rules.

---

_Comment by @flying-sheep on 2024-07-05 08:51_

I’m sorry! :sweat_smile:

---

_Label `bug` added by @MichaReiser on 2024-07-05 08:58_

---

_Comment by @MichaReiser on 2024-07-05 09:15_

Okay, I think the problem is that the following code only runs between statements:

https://github.com/astral-sh/ruff/blob/549cc1e437479ec7fde0dcf3e7360fb95d06076d/crates/ruff_python_formatter/src/statement/suite.rs#L165-L216

But the code path is not executed before a clause header. 

---

_Label `breaking` added by @MichaReiser on 2024-07-05 09:33_

---

_Comment by @MichaReiser on 2024-07-05 09:34_

Okay, I don't think we can fix this bug right now because it would change formatting of existing code. 


My naive approach of duplicating some of the logic into the `FormatClauseHeader` causes instability. We need to look into this more closely when working on the next preview style

```patch
Index: crates/ruff_python_formatter/src/statement/clause.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_python_formatter/src/statement/clause.rs b/crates/ruff_python_formatter/src/statement/clause.rs
--- a/crates/ruff_python_formatter/src/statement/clause.rs	(revision 1e07bfa3730db9461f51b877bf71ea31e7dd56e4)
+++ b/crates/ruff_python_formatter/src/statement/clause.rs	(date 1720171838242)
@@ -1,5 +1,5 @@
 use ruff_formatter::{write, Argument, Arguments, FormatError};
-use ruff_python_ast::AnyNodeRef;
+use ruff_python_ast::{AnyNodeRef, Stmt};
 use ruff_python_ast::{
     ElifElseClause, ExceptHandlerExceptHandler, MatchCase, StmtClassDef, StmtFor, StmtFunctionDef,
     StmtIf, StmtMatch, StmtTry, StmtWhile, StmtWith, Suite,
@@ -23,7 +23,10 @@
     Class(&'a StmtClassDef),
     Function(&'a StmtFunctionDef),
     If(&'a StmtIf),
-    ElifElse(&'a ElifElseClause),
+    ElifElse {
+        clause: &'a ElifElseClause,
+        last_prev_body_statement: Option<&'a Stmt>,
+    },
     Try(&'a StmtTry),
     ExceptHandler(&'a ExceptHandlerExceptHandler),
     TryFinally(&'a StmtTry),
@@ -47,7 +50,7 @@
         let end = match self {
             ClauseHeader::Class(class) => Some(last_child_end.unwrap_or(class.name.end())),
             ClauseHeader::Function(function) => Some(last_child_end.unwrap_or(function.name.end())),
-            ClauseHeader::ElifElse(_)
+            ClauseHeader::ElifElse { .. }
             | ClauseHeader::Try(_)
             | ClauseHeader::If(_)
             | ClauseHeader::TryFinally(_)
@@ -125,11 +128,15 @@
             }) => {
                 visit(test.as_ref(), visitor);
             }
-            ClauseHeader::ElifElse(ElifElseClause {
-                test,
-                range: _,
-                body: _,
-            }) => {
+            ClauseHeader::ElifElse {
+                clause:
+                    ElifElseClause {
+                        test,
+                        range: _,
+                        body: _,
+                    },
+                ..
+            } => {
                 if let Some(test) = test.as_ref() {
                     visit(test, visitor);
                 }
@@ -220,14 +227,21 @@
                 find_keyword(start_position, keyword, source)
             }
             ClauseHeader::If(header) => find_keyword(header.start(), SimpleTokenKind::If, source),
-            ClauseHeader::ElifElse(ElifElseClause {
-                test: None, range, ..
-            }) => find_keyword(range.start(), SimpleTokenKind::Else, source),
-            ClauseHeader::ElifElse(ElifElseClause {
-                test: Some(_),
-                range,
+            ClauseHeader::ElifElse {
+                clause: ElifElseClause {
+                    test: None, range, ..
+                },
+                ..
+            } => find_keyword(range.start(), SimpleTokenKind::Else, source),
+            ClauseHeader::ElifElse {
+                clause:
+                    ElifElseClause {
+                        test: Some(_),
+                        range,
+                        ..
+                    },
                 ..
-            }) => find_keyword(range.start(), SimpleTokenKind::Elif, source),
+            } => find_keyword(range.start(), SimpleTokenKind::Elif, source),
             ClauseHeader::Try(header) => find_keyword(header.start(), SimpleTokenKind::Try, source),
             ClauseHeader::ExceptHandler(header) => {
                 find_keyword(header.start(), SimpleTokenKind::Except, source)
@@ -347,6 +361,73 @@
 
 impl<'ast> Format<PyFormatContext<'ast>> for FormatClauseHeader<'_, 'ast> {
     fn fmt(&self, f: &mut Formatter<PyFormatContext<'ast>>) -> FormatResult<()> {
+        match self.header {
+            ClauseHeader::ElifElse {
+                clause: elif,
+                last_prev_body_statement,
+            } => {
+                let needs_empty_line = {
+                    // Find nested class or function definitions that need an empty line after them.
+                    //
+                    // ```python
+                    // def f():
+                    //     if True:
+                    //
+                    //         def double(s):
+                    //             return s + s
+                    //
+                    //     print("below function")
+                    // ```
+                    std::iter::successors(
+                        last_prev_body_statement.map(AnyNodeRef::from),
+                        AnyNodeRef::last_child_in_body,
+                    )
+                    .take_while(|last_child|
+                        // If there is a comment between preceding and following the empty lines were
+                        // inserted before the comment by preceding and there are no extra empty lines
+                        // after the comment.
+                        // ```python
+                        // class Test:
+                        //     def a(self):
+                        //         pass
+                        //         # trailing comment
+                        //
+                        //
+                        // # two lines before, one line after
+                        //
+                        // c = 30
+                        // ````
+                        // This also includes nested class/function definitions, so we stop recursing
+                        // once we see a node with a trailing own line comment:
+                        // ```python
+                        // def f():
+                        //     if True:
+                        //
+                        //         def double(s):
+                        //             return s + s
+                        //
+                        //         # nested trailing own line comment
+                        //     print("below function with trailing own line comment")
+                        // ```
+                        !f.context().comments().has_trailing_own_line(*last_child))
+                    .any(|last_child| {
+                        matches!(
+                            last_child,
+                            AnyNodeRef::StmtFunctionDef(_) | AnyNodeRef::StmtClassDef(_)
+                        )
+                    })
+                };
+
+                if needs_empty_line {
+                    empty_line().fmt(f)?;
+                }
+            }
+            ClauseHeader::ExceptHandler(handler) => {}
+            ClauseHeader::TryFinally(finally) => {}
+            // ClauseHeader::OrElse(else_clause)
+            _ => {}
+        }
+
         if let Some((leading_comments, last_node)) = self.leading_comments {
             leading_alternate_branch_comments(leading_comments, last_node).fmt(f)?;
         }
Index: crates/ruff_python_formatter/src/statement/stmt_if.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_python_formatter/src/statement/stmt_if.rs b/crates/ruff_python_formatter/src/statement/stmt_if.rs
--- a/crates/ruff_python_formatter/src/statement/stmt_if.rs	(revision 1e07bfa3730db9461f51b877bf71ea31e7dd56e4)
+++ b/crates/ruff_python_formatter/src/statement/stmt_if.rs	(date 1720171815065)
@@ -1,5 +1,5 @@
 use ruff_formatter::{format_args, write};
-use ruff_python_ast::AnyNodeRef;
+use ruff_python_ast::{AnyNodeRef, Stmt};
 use ruff_python_ast::{ElifElseClause, StmtIf};
 use ruff_text_size::Ranged;
 
@@ -39,10 +39,10 @@
             ]
         )?;
 
-        let mut last_node = body.last().unwrap().into();
+        let mut last_node = body.last().unwrap();
         for clause in elif_else_clauses {
             format_elif_else_clause(clause, f, Some(last_node))?;
-            last_node = clause.body.last().unwrap().into();
+            last_node = clause.body.last().unwrap();
         }
 
         Ok(())
@@ -54,7 +54,7 @@
 pub(crate) fn format_elif_else_clause(
     item: &ElifElseClause,
     f: &mut PyFormatter,
-    last_node: Option<AnyNodeRef>,
+    last_node: Option<&Stmt>,
 ) -> FormatResult<()> {
     let ElifElseClause {
         range: _,
@@ -70,7 +70,10 @@
         f,
         [
             clause_header(
-                ClauseHeader::ElifElse(item),
+                ClauseHeader::ElifElse {
+                    clause: item,
+                    last_prev_body_statement: last_node
+                },
                 trailing_colon_comment,
                 &format_with(|f: &mut PyFormatter| {
                     f.options()
Index: crates/ruff_python_formatter/src/lib.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_python_formatter/src/lib.rs b/crates/ruff_python_formatter/src/lib.rs
--- a/crates/ruff_python_formatter/src/lib.rs	(revision 1e07bfa3730db9461f51b877bf71ea31e7dd56e4)
+++ b/crates/ruff_python_formatter/src/lib.rs	(date 1720169741546)
@@ -188,13 +188,13 @@
     #[test]
     fn quick_test() {
         let source = r#"
-def main() -> None:
-    if True:
-        some_very_long_variable_name_abcdefghijk = Foo()
-        some_very_long_variable_name_abcdefghijk = some_very_long_variable_name_abcdefghijk[
-            some_very_long_variable_name_abcdefghijk.some_very_long_attribute_name
-            == "This is a very long string abcdefghijk"
-        ]
+def test():
+    if header[0] == "Package":
+        def strengthen(k: str) -> str:
+            return f"<strong>{k}</strong>"
+    else:
+        pass
+
 
 "#;
         let source_type = PySourceType::Python;
```

---

_Assigned to @konstin by @konstin on 2024-07-10 09:12_

---

_Closed by @konstin on 2024-07-15 10:59_

---

_Closed by @konstin on 2024-07-15 10:59_

---

_Comment by @flying-sheep on 2024-07-20 10:09_

@konstin thank you!

btw. the world is small: you’re in Munich, I’m in Munich, and I’m just leaving a conference in China where I met fellow people from Munich.

---
