```yaml
number: 6336
title: Autofix for UP032 swallows comment
type: issue
state: closed
author: WhyNotHugo
labels:
  - fixes
  - needs-decision
assignees: []
created_at: 2023-08-04T09:42:06Z
updated_at: 2023-08-04T15:50:36Z
url: https://github.com/astral-sh/ruff/issues/6336
synced_at: 2026-01-12T15:54:46Z
```

# Autofix for UP032 swallows comment

---

_@WhyNotHugo_

Given this snippet:

```py
        return "data:image/{};base64,{}".format(
            ext[1:], data.decode()  # Remove the leading dot.
        )
```

Ruff autofix produces this output:

```py
        return f"data:image/{ext[1:]};base64,{data.decode()}"
```

Note that the autofixed code "swallows" the comment. See here for original context: https://github.com/WhyNotHugo/django-afip/pull/186/commits/2974184b776bede851eba0632b80d915673dea81

This is reproducible with ruff 0.0.281. [The relevant pyproject.toml is here](https://github.com/WhyNotHugo/django-afip/blob/2974184b776bede851eba0632b80d915673dea81/pyproject.toml).

---

_Comment by @harupy on 2023-08-04 12:32_

@charliermarsh Can I work on fixing this?

---

_Comment by @charliermarsh on 2023-08-04 13:29_

Sure. How are you thinking of resolving? What's the expected behavior here?

---

_Label `autofix` added by @charliermarsh on 2023-08-04 13:29_

---

_Label `needs-decision` added by @charliermarsh on 2023-08-04 13:29_

---

_Comment by @harupy on 2023-08-04 13:31_

```diff
diff --git a/crates/ruff/src/rules/pyupgrade/rules/f_strings.rs b/crates/ruff/src/rules/pyupgrade/rules/f_strings.rs
index 1739207cb..2dbf10061 100644
--- a/crates/ruff/src/rules/pyupgrade/rules/f_strings.rs
+++ b/crates/ruff/src/rules/pyupgrade/rules/f_strings.rs
@@ -320,7 +320,7 @@ pub(crate) fn f_strings(
         return;
     };
 
-    let Expr::Attribute(ast::ExprAttribute { value, .. }) = func.as_ref() else {
+    let Expr::Attribute(ast::ExprAttribute { value, attr, .. }) = func.as_ref() else {
         return;
     };
 
@@ -334,6 +334,25 @@ pub(crate) fn f_strings(
         return;
     };
 
+    // Exit if comments are present in the argument range:
+    // ```
+    // "{} {}".format(
+    //   1,  # 1
+    //   2,  # 2
+    // )
+    // ```
+    if lexer::lex(
+        checker
+            .locator()
+            .slice(TextRange::new(attr.start(), expr.end())),
+        Mode::Expression,
+    )
+    .flatten()
+    .any(|(tok, _)| matches!(tok, Tok::Comment(..)))
+    {
+        return;
+    }
+
     let Some(mut summary) = FormatSummaryValues::try_from_expr(expr, checker.locator()) else {
         return;
     };
```

I think preserving comments is not easy. Can we avoid raising UP032 if comments are present in the argument range like this^?

---

_Comment by @WhyNotHugo on 2023-08-04 13:33_

> Can we avoid raising UP032 if comments are present in the argument range like this^?

That's definitely one approach. Another possible approach is to raise the warning but skip it when auto-fixing.

I'll let you decide which one is best :)

---

_Comment by @charliermarsh on 2023-08-04 13:45_

Yeah I'd say raise-but-not-fix if there are comments within the call.

---

_Comment by @harupy on 2023-08-04 13:47_

+1 to raise-but-not-fix.

---

_Closed by @charliermarsh on 2023-08-04 15:37_

---

_Comment by @WhyNotHugo on 2023-08-04 15:50_

That was quick! Thanks

---
