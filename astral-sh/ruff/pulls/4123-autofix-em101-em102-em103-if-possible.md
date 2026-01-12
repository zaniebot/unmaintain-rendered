```yaml
number: 4123
title: "Autofix `EM101`, `EM102`, `EM103` if possible"
type: pull_request
state: merged
author: dhruvmanila
labels: []
assignees: []
merged: true
base: main
head: autofix/flake8-err-msg
created_at: 2023-04-26T19:08:16Z
updated_at: 2023-04-28T15:18:06Z
url: https://github.com/astral-sh/ruff/pull/4123
synced_at: 2026-01-12T04:28:19Z
```

# Autofix `EM101`, `EM102`, `EM103` if possible

---

_Pull request opened by @dhruvmanila on 2023-04-26 19:08_

## Summary

Generate an autofix for `EM101`, `EM102`, `EM103` if possible. This will avoid the autofix if the `msg` variable is bound in the current scope or the parent scope. Otherwise, it'll produce 2 edits for the fix:
1. Insert the exception argument into a variable assignment before the `raise` statement. The variable name is `msg`.
2. Replace the exception argument with the variable name.

**Indentation** is taken care using the helper function [`ruff_python_ast::whitespace::indentation`](https://github.com/charliermarsh/ruff/blob/4356d8e1648588523aa8617d9deb631b63ca1c42/crates/ruff_python_ast/src/whitespace.rs#L7-L7)
**Newline** is taken from [`checker.stylist.line_ending`](https://github.com/charliermarsh/ruff/blob/4356d8e1648588523aa8617d9deb631b63ca1c42/crates/ruff_python_ast/src/source_code/stylist.rs#L23-L23)

fixes: #4103 

---

_Renamed from "Autofix `EM001`, `EM002`, `EM003` if possible" to "Autofix `EM101`, `EM102`, `EM103` if possible" by @dhruvmanila on 2023-04-26 19:51_

---

_@charliermarsh reviewed on 2023-04-27 04:21_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/flake8_errmsg/EM.py`:47 on 2023-04-27 04:21_

Can we ensure that we also properly handle non-indented statements? (We could even consider these unfixable, but the code _might_ panic if we're unwrapping on indentation right now.)

```
if foo: raise RuntimeError("This is an example exception")
if foo: x = 1; raise RuntimeError("This is an example exception")
```

---

_@charliermarsh reviewed on 2023-04-27 04:21_

Looks great, just one question below.

---

_@dhruvmanila reviewed on 2023-04-27 04:36_

---

_Review comment by @dhruvmanila on `crates/ruff/resources/test/fixtures/flake8_errmsg/EM.py`:47 on 2023-04-27 04:36_

Yes, it does panic.

I think what we could do is to add the newline _before_ the assignment statement (currently it's _after_ the statement), but only if the indentation is not present. If the indentation is not present, then we'll use the default indentation (`checker.stylist.indentation`).

I'll test this out, otherwise we could just make it unfixable.

---

_@dhruvmanila reviewed on 2023-04-27 13:53_

---

_Review comment by @dhruvmanila on `crates/ruff/resources/test/fixtures/flake8_errmsg/EM.py`:47 on 2023-04-27 13:53_

It's actually unfixable -- the logic might work for the first statement, but it'll fail when there are multiple statements on the same line.

---

_Comment by @dhruvmanila on 2023-04-27 14:08_

Ecosystem checks can be found here: https://github.com/charliermarsh/ruff/actions/runs/4820598417/attempts/1#summary-13071431799

They're just addition of the `[*]` prefix before the message.

---

_Comment by @charliermarsh on 2023-04-27 18:47_

As a stretch, it would be nice to parenthesize expressions in cases like this (imagine the expression is actually multi-line):

```diff
+            msg = "Some very long message"
             raise Exception(
-                "Some very long "
-                "message"
+                msg
             )
```

---

_@charliermarsh reviewed on 2023-04-27 18:48_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_errmsg/rules.rs`:239 on 2023-04-27 18:48_

@dhruvmanila - I'm not convinced that the change I made here is _clearer_, but it does mean that we avoid the "invalid" state of `fixable = false` with `indentation = ""`.

---

_Merged by @charliermarsh on 2023-04-27 18:53_

---

_Closed by @charliermarsh on 2023-04-27 18:53_

---

_Comment by @dhruvmanila on 2023-04-28 11:35_

> As a stretch, it would be nice to parenthesize expressions in cases like this (imagine the expression is actually multi-line):

Do you mean to autofix it like so?
 
```diff
+msg = (
+    "This is an example exception. "
+    "This message is very long "
+    "and spans multiple lines."
+)
 raise Exception(
-    "This is an example exception. "
-    "This message is very long "
-    "and spans multiple lines."
+    msg
 )
```

It could be but not with the help of AST. I think slicing the source code would work with `msg = {content}` as the replacement string.



---

_@dhruvmanila reviewed on 2023-04-28 11:47_

---

_Review comment by @dhruvmanila on `crates/ruff/src/rules/flake8_errmsg/rules.rs`:239 on 2023-04-28 11:47_

Ah, right. Yes, it would seem that the fixable property is dependent on whether the indentation is present or not. Maybe two variables might help but not the way I did it. 🤔

```diff
diff --git a/crates/ruff/src/rules/flake8_errmsg/rules.rs b/crates/ruff/src/rules/flake8_errmsg/rules.rs
index ac30f0d7..e828e416 100644
--- a/crates/ruff/src/rules/flake8_errmsg/rules.rs
+++ b/crates/ruff/src/rules/flake8_errmsg/rules.rs
@@ -229,20 +229,11 @@ pub fn string_in_exception(checker: &mut Checker, stmt: &Stmt, exc: &Expr) {
                 } => {
                     if checker.settings.rules.enabled(Rule::RawStringInException) {
                         if string.len() > checker.settings.flake8_errmsg.max_string_length {
-                            let indentation = whitespace::indentation(checker.locator, stmt)
-                                .and_then(|indentation| {
-                                    if checker.ctx.find_binding("msg").is_none() {
-                                        Some(indentation)
-                                    } else {
-                                        None
-                                    }
-                                });
-                            let mut diagnostic = Diagnostic::new(
-                                RawStringInException {
-                                    fixable: indentation.is_some(),
-                                },
-                                first.range(),
-                            );
+                            let indentation = whitespace::indentation(checker.locator, stmt);
+                            let fixable = indentation
+                                .map_or(false, |_| checker.ctx.find_binding("msg").is_none());
+                            let mut diagnostic =
+                                Diagnostic::new(RawStringInException { fixable }, first.range());
                             if let Some(indentation) = indentation {
                                 if checker.patch(diagnostic.kind.rule()) {
                                     diagnostic.set_fix(generate_fix(
```
 

---

_Branch deleted on 2023-04-28 11:48_

---
