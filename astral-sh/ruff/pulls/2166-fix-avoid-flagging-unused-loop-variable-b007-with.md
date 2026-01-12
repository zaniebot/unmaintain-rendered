```yaml
number: 2166
title: "fix: avoid flagging unused loop variable (B007) with globals(), vars() or eval()"
type: pull_request
state: merged
author: spaceone
labels: []
assignees: []
merged: true
base: main
head: 2161-unused-variable-globals-vars
created_at: 2023-01-25T18:56:56Z
updated_at: 2023-01-25T20:19:14Z
url: https://github.com/astral-sh/ruff/pull/2166
synced_at: 2026-01-12T04:52:00Z
```

# fix: avoid flagging unused loop variable (B007) with globals(), vars() or eval()

---

_Pull request opened by @spaceone on 2023-01-25 18:56_

see issue #2161

maybe even the use of `exec()` could be a case?

and what do you think about:
```diff
diff --git src/rules/flake8_bugbear/rules/unused_loop_control_variable.rs src/rules/flake8_bugbear/rules/unused_loop_control_variable.rs
index 7783c7a4..e4a453ab 100644
--- src/rules/flake8_bugbear/rules/unused_loop_control_variable.rs
+++ src/rules/flake8_bugbear/rules/unused_loop_control_variable.rs
@@ -57,10 +57,6 @@ where
 
 /// B007
 pub fn unused_loop_control_variable(checker: &mut Checker, target: &Expr, body: &[Stmt]) {
-    if helpers::uses_magic_variable_access(body) {
-        return;
-    }
-
     let control_names = {
         let mut finder = NameFinder::new();
         finder.visit_expr(target);
@@ -90,7 +86,7 @@ pub fn unused_loop_control_variable(checker: &mut Checker, target: &Expr, body:
             violations::UnusedLoopControlVariable(name.to_string()),
             Range::from_located(expr),
         );
-        if checker.patch(diagnostic.kind.rule()) {
+        if !helpers::uses_magic_variable_access(body) && checker.patch(diagnostic.kind.rule()) {
             // Prefix the variable name with an underscore.
             diagnostic.amend(Fix::replacement(
                 format!("_{name}"),
```
so that it's still noted but just not fixed (untested)?

---

_Merged by @charliermarsh on 2023-01-25 20:18_

---

_Closed by @charliermarsh on 2023-01-25 20:18_

---

_Comment by @charliermarsh on 2023-01-25 20:19_

Sounds good. I went ahead and added `exec`, and changed the code to lint but not fix if the fix is "unsafe".

---
