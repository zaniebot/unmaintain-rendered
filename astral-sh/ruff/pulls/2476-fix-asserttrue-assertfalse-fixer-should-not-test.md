```yaml
number: 2476
title: "fix: assertTrue()/assertFalse() fixer should not test for identity"
type: pull_request
state: merged
author: spaceone
labels:
  - bug
assignees: []
merged: true
base: main
head: 2456-assert-false
created_at: 2023-02-02T13:33:48Z
updated_at: 2023-02-02T16:24:36Z
url: https://github.com/astral-sh/ruff/pull/2476
synced_at: 2026-01-12T15:55:08Z
```

# fix: assertTrue()/assertFalse() fixer should not test for identity

---

_@spaceone_

Issue #2456

`cargo clippy` wants this but then `_expr` is unused:
```diff
diff --git src/rules/flake8_pytest_style/rules/unittest_assert.rs src/rules/flake8_pytest_style/rules/unittest_assert.rs
index 29f7ab44..35beaeca 100644
--- src/rules/flake8_pytest_style/rules/unittest_assert.rs
+++ src/rules/flake8_pytest_style/rules/unittest_assert.rs
@@ -266,12 +266,12 @@ impl UnittestAssert {
                     .ok_or_else(|| anyhow!("Missing argument `expr`"))?;
                 let msg = args.get("msg").copied();
                 if matches!(self, UnittestAssert::True) {
-                    let expr = create_expr(ExprKind::UnaryOp {
+                    let _expr = create_expr(ExprKind::UnaryOp {
                         op: Unaryop::Not,
                         operand: Box::new(create_expr(expr.node.clone())),
                     });
                 }
-                Ok(assert(&expr, msg))
+                Ok(assert(expr, msg))
             }
             UnittestAssert::Equal
             | UnittestAssert::Equals
```

strange...

---

_Comment by @charliermarsh on 2023-02-02 13:48_

If you create a binding like that, it doesn't live beyond the curly braces.

```rs
let x = 1;
if true {
  let x = 2;
}
println!("{}", x);
```

This should print `1`, if I'm not mistaken, since the binding is scoped to the `{}`. So in the example above, the inner `let expr` is unused!

---

_Comment by @spaceone on 2023-02-02 14:07_

oh, good to know! fixed.

---

_Review comment by @edgarrmondragon on `src/rules/flake8_pytest_style/rules/unittest_assert.rs`:275 on 2023-02-02 15:50_

I think you want to swap these

```suggestion
                    Ok(assert(expr, msg))
                } else {
                    let unary_expr = create_expr(ExprKind::UnaryOp {
                        op: Unaryop::Not,
                        operand: Box::new(create_expr(expr.node.clone())),
                    });
                    Ok(assert(&unary_expr, msg))
```

---

_@edgarrmondragon reviewed on 2023-02-02 15:50_

---

_@spaceone reviewed on 2023-02-02 16:10_

---

_Review comment by @spaceone on `src/rules/flake8_pytest_style/rules/unittest_assert.rs`:275 on 2023-02-02 16:10_

correct, thanks! I adjusted this.

---

_Label `bug` added by @charliermarsh on 2023-02-02 16:17_

---

_Merged by @charliermarsh on 2023-02-02 16:24_

---

_Closed by @charliermarsh on 2023-02-02 16:24_

---
